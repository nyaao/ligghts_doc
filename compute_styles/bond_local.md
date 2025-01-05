# compute bond/local command

## 構文
```
compute ID group-ID bond/local general_keyword general_values input1 input2 ...
```
- ID、group-ID は compute コマンドで文書化されています。
- bond/local = この compute コマンドのスタイル名
- general_keywords general_values は compute コマンドで文書化されています。
- 1つ以上のキーワードを追加できます。
- キーワード = dist または eng

dist = 結合距離  
eng = 結合エネルギー  
force = 結合力  

## 例
```
compute 1 all bond/local eng
compute 1 all bond/local dist eng force
```

## 説明
個々の結合相互作用の特性を計算する計算を定義します。生成されるデータの数は、システム内の結合の数に等しく、以下で説明するようにグループパラメータによって修正されます。

このコマンドによって保存されるローカルデータは、プロセッサ上で所有されているすべての原子とその結合をループ処理することによって生成されます。結合は、結合内の両方の原子が指定された計算グループに含まれている場合にのみ含まれます。結合タイプが0に設定されている（[bond_style]() コマンドで設定された）切断された結合は含まれません。結合タイプが負に設定された（fix shake や [delete_bonds]() コマンドで設定された）無効化された結合はファイルに書き込まれますが、そのエネルギーは 0.0 になります。

原子がプロセッサ間で移動する際、ローカルベクトルや配列内のエントリの順序は、あるタイムステップから次のタイムステップへ一貫していないことに注意してください。保証される唯一の一貫性は、特定のタイムステップにおける順序が、他の compute コマンドによって生成されたローカルベクトルや配列においても同じであるという点です。たとえば、[compute property/local]() コマンドからの結合出力は、このコマンドからのデータと組み合わせて、dump local コマンドによって一貫した方法で出力することができます。

以下は、その方法の例です。
```
compute 1 all property/local batom1 batom2 btype
compute 2 all bond/local dist eng
dump 1 all local 1000 tmp.dump index c_1[1] c_1[2] c_1[3] c_2[1] c_2[2]
```

## 出力情報
この compute は、キーワードの数に応じてローカルベクトルまたはローカル配列を計算します。ベクトルの長さまたは配列の行数は結合の数です。キーワードが1つだけ指定されている場合、ローカルベクトルが生成されます。キーワードが2つ以上指定されている場合、ローカル配列が生成され、列数はキーワードの数と等しくなります。ベクトルまたは配列は、compute からのローカル値を入力として使用する任意のコマンドでアクセスできます。LIGGGHTS(R)-PUBLIC の出力オプションの概要については、このセクションを参照してください。

dist の出力は距離の[units]()で表示されます。eng の出力はエネルギーの[units]()で表示されます。force の出力は力の[units]()で表示されます。

## 制限事項
none

## 関連コマンド
[dump local](), [compute property/local]()

## デフォルト
none
