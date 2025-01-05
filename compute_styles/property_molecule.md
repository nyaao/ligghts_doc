# compute property/molecule command

## 構文
```
compute ID group-ID property/molecule general_keyword general_values input1 input2 ...
```

- ID、group-IDはcomputeコマンドで記録されています。
- property/molecule = このcomputeコマンドのスタイル名
- general_keywords general_valuesはcomputeで記録されています。
- input = 1つ以上の属性

可能な属性 = mol, cout
mol = 分子ID
count = 分子内の原子の数

## 例
```
compute 1 all property/molecule mol
```

## 説明
この計算は、指定された属性をグローバルデータとして格納し、他の[output command]()や、[compute com/molecule]() や [compute msd/molecule]() など、分子単位のデータを生成する他のコマンドと組み合わせて使用できるようにします。

この計算によって生成される分子単位の量の順序は、分子単位のデータを生成する他の計算コマンドで生成された順序と一致します。概念的には、指定されたグループに1つ以上の原子が含まれる分子については、分子IDが昇順になります。

mol 属性は分子IDです。この属性は、他の計算やフィックスが出力ファイルに出力する際に、分子単位のデータにラベルとして分子IDを生成するために使用できます（例えば、fix ave/time コマンドによる出力）。

count 属性は分子内の原子の数です。

## 出力情報
この計算は、指定された入力値の数に応じて、グローバルベクトルまたはグローバル配列を計算します。ベクトルの長さまたは配列の行数は分子の数です。1つの入力が指定された場合、グローバルベクトルが生成されます。2つ以上の入力が指定された場合、グローバル配列が生成され、列の数は入力の数と等しくなります。このベクトルまたは配列は、計算からのグローバル値を入力として使用する任意のコマンドからアクセスできます。LIGGGHTS(R)-PUBLICの出力オプションの概要については、このセクションを参照してください。

ベクトルまたは配列の値は、指定された属性に対応する整数値です。

## 制限事項
none

## 関連コマンド
none

## デフォルト
none