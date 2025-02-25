# fix bond/break command

# 構文
```
fix ID group-ID bond/break Nevery bondtype Rmax keyword values ...
```
- ID と group-ID は fix コマンドで記述されています。
- bond/break = この修正コマンドのスタイル名
- Nevery = 指定したステップ間隔ごとに結合の破壊を試みる
- bondtype = 破壊対象となる結合の種類
- Rmax = この距離（単位：距離）を超える結合は破壊される可能性がある
- 0個以上のキーワード/値のペアを引数に追加できます。
- keyword = prob

prob の値 = fraction seed  
fraction = 条件を満たす場合に、結合をこの確率で破壊する  
seed = 乱数シード（10,000より大きい素数）

# 例
```
fix 5 all bond/break 10 2 1.2
fix 5 polymer bond/break 1 1 2.0 prob 0.5 123457
```

# 説明
シミュレーションが進行する中で、指定された基準に基づいて原子対間の結合を破壊します。これは、シミュレーションボックスの伸長やその他の変形によるポリマーネットワークの解体をモデル化するために使用できます。この文脈での「結合」とは、[bond_style]() コマンドによって計算される原子対間の相互作用を意味します。一度結合が破壊されると、それは永久に削除されます。これは、TersoffやAIREBOのような[pairwize]()の結合次数ポテンシャルとは異なり、小さな原子クラスターの現在の幾何構造に基づいて結合と多体相互作用を推測し、原子が移動するにつれてタイムステップごとに結合を実質的に作成および破壊するものです。

結合破壊の可能性についてのチェックは、Nevery タイムステップごとに行われます。もし結合された2つの原子 I と J が距離 Rmax を超えて離れている場合、結合の種類が指定された bondtype であり、かつ I と J が指定された修正グループに属している場合、I と J の結合は「破壊の可能性がある」結合としてラベル付けされます。

1つの原子が複数の引き伸ばされた結合を持つ場合、その原子は破壊の可能性がある結合リストを確認し、最も長い結合をその原子の「唯一の」破壊対象結合としてラベル付けします。この処理の後、もし原子 I が原子 J と「唯一の」結合を持ち、かつ原子 J も原子 I と「唯一の」結合を持つ場合、I と J の結合は「破壊の対象」として「適格」となります。

これらのルールにより、1つの原子が特定のタイムステップで破壊される結合に1つだけ関与することになります。また、もし原子 I が原子 J を唯一の結合相手として選択した場合でも、原子 J が原子 K を唯一の結合相手として選択した場合（Rjk > Rij のため）、原子 I はこのタイムステップで破壊される結合には関与しないことを意味します。たとえ他に破壊の可能性がある結合相手が存在していても同様です。

prob キーワードを使用すると、適格な結合が実際に破壊されるかどうかに影響を与えます。fraction の設定値は 0.0 ～ 1.0 の範囲でなければなりません。一様分布に従う乱数が 0.0 ～ 1.0 の間で生成され、その乱数が fraction より小さい場合にのみ、適格な結合が破壊されます。

結合が破壊されると、LIGGGHTS(R)-PUBLIC 内部の結合トポロジーを格納するデータ構造が更新され、破壊が反映されます。これにより、結合内の原子が関与するペアワイズ相互作用のその後の計算にも影響を及ぼす可能性があります。追加の情報については、以下の「制約」セクションを参照してください。

計算上、この fix が動作する各タイムステップでは、結合リストをループして、リスト内で結合している原子ペア間の距離を計算します。また、どの結合が破壊されるかを調整するために、隣接するプロセッサ間で通信も行います。そのため、タイムステップの計算コストが増加します。この fix をあまり頻繁に呼び出すと、計算コストが高くなるため注意が必要です。

現在の結合トポロジーのスナップショットを [dump local]() コマンドを使用して出力することができます。

[!WARNING]結合を破壊すると、通常、システムのエネルギーが変化します。そのため、エネルギーに大きな変化を引き起こすような結合破壊の条件を設定しないよう注意する必要があります。例えば、非常に剛性の高い調和結合を定義し、2つの原子が平衡結合長から大きく離れた距離で結合を破壊するよう設定した場合、結合が破壊された際にその2つの原子が劇的に解放されることになります。より一般的には、破壊された結合によるエネルギー変化を補うために、システムにサーモスタットを適用する必要があるかもしれません。

# Restart, fix_modify, output, run start/stop, minimize info
この fix に関する情報はバイナリ再スタートファイルには書き込まれません。また、fix_modify オプションはこの fix には適用されません。

この fix は、2つの統計量を計算し、それを長さ2のグローバルベクトルとして保存します。このベクトルは、さまざまな出力コマンドからアクセス可能です。この fix によって計算されるベクトル値は「集約量（intensive）」です。

以下がその2つの統計量です：

1. 直近の破壊タイムステップで破壊された結合の数
1. 累積で破壊された結合の数

この fix のパラメータは、run コマンドの start/stop キーワードと併用することはできません。また、この fix はエネルギー最小化（minimize コマンド）中には実行されません。

# 制限事項
この fix は MC パッケージ の一部であり、LIGGGHTS(R)-PUBLIC がそのパッケージでビルドされている場合にのみ有効になります。詳細は「Making LIGGGHTS(R)-PUBLIC」のセクションを参照してください。

現在、この fix を使用する際には以下の2つの制約があります。将来的に新しいモデルが追加されれば、これらの制約が緩和される可能性があります。

結合が破壊された際の角度および二面角の相互作用について
結合が破壊された場合、関連する角度や二面角の相互作用を無効にしたいと考えるかもしれません。しかし、LIGGGHTS(R)-PUBLIC では、angle_style や dihedral_style が定義されている場合でも、これらの角度や二面角をチェックしません。

special_bonds コマンドで定義されるペアワイズ重み付けについて
この fix では、結合トポロジー内の 1-2、1-3、および 1-4 の隣接原子に対するペアワイズ重み付けが 0,1,1 である必要があります。これにより、結合が存在している間は I と J 間のペアワイズ相互作用が無効化され、結合が破壊されると有効化されます。また、I と J の他の結合パートナーとのペアワイズ相互作用は、その結合の存在によって影響を受けません。

# 関連コマンド
[fix bond/create](), [fix bond/swap](), [dump local](), [special_bonds]()

# デフォルト
オプションのデフォルトは、prob = 1.0 です。