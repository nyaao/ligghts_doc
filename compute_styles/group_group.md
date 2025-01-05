# compute group/group command

## 構文
```
compute ID group-ID group/group general_keyword general_values group2-ID keyword value ...
```
- ID、group-IDはcomputeコマンドに記載されています。
- group/group = この計算コマンドのスタイル名です。
- general_keywords general_valuesはcomputeに記載されています。
- group2-ID = 2番目（または同じ）のグループのgroup IDです。
- 0個以上のキーワード/値のペアを追加することができます。
- キーワード = pair または boundary
pair value = yes または no
boundary value = yes または no

## 例
```
compute 1 lower group/group upper
compute mine fluid group/group wall
```

## 説明
計算を定義し、2つの原子群（computeグループと指定されたgroup2）間の総エネルギーと力の相互作用を計算します。2つのグループは同じであってもかまいません。

もしpairキーワードが「yes」に設定されていれば（デフォルト設定）、相互作用エネルギーにはペアコンポーネントが含まれ、これはペアとなるすべての原子間のエネルギーとして定義されます。ペアの一方の原子が最初のグループにあり、もう一方が2番目のグループにある場合です。同様に、この計算で得られる相互作用の力には、computeグループの原子に対する指定されたgroup2の原子とのペア相互作用による力が含まれます。

この計算では、2つのグループの間のボンド相互作用は計算されません。

グループ間のペアワイズな相互作用の貢献は、近隣リストをループ処理することによって計算されます。

## 出力情報
この計算は、グローバルスカラー（エネルギー）と長さ3のグローバルベクトル（力）を計算します。これらの値は、インデックス1〜3でアクセスできます。これらの値は、計算の出力からグローバルスカラーやベクトルの値を入力として使用するコマンドで利用できます。このセクションでLIGGGHTS(R)-PUBLICの出力オプションの概要をご覧ください。

この計算によって得られたスカラーとベクトルの値は「広がりのある（extensive）」値です。スカラー値はエネルギー単位で、ベクトル値は力の単位で表されます。

## 制限事項
none

## 関連コマンド
none

## デフォルト
オプションのデフォルトは pair = yes, kspace = no, and boundary = yes です。