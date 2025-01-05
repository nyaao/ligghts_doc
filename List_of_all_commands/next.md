# next command

## 構文
```
next variables
```
variables = 1つ以上の変数名

## 例
```
next x
next a t x myTemp
```

## 説明
このコマンドは、[variable]() コマンドで定義された変数と共に使用されます。[variable]() コマンドで定義された値のリストから次の値を変数に割り当てます。これにより、その変数が後で入力スクリプトコマンドで置き換えられる際に、新しい値が使用されます。

LIGGGHTS(R)-PUBLIC の入力スクリプトで異なる種類の変数を定義し使用する方法については、[variable]() コマンドの説明を参照してください。もし変数名が「a」から「z」までの小文字の1文字であれば、入力スクリプトコマンド内で $a や $z として使用できます。複数の文字の場合は、${myTemp} のように使用できます。

次のコマンドに複数の変数を引数として使用する場合、すべて同じ変数スタイル（index、loop、file、universe、または uloop）である必要があります。ただし、universe スタイルと uloop スタイルの変数は、同じ次のコマンド内で混在させることができます。

next コマンドで指定されたすべての変数は、それぞれの値のリストから1つずつ値がインクリメントされます。file スタイルの変数は、関連付けられたファイルから次の行を読み取ります。atomfile スタイルの変数は、関連付けられたファイルから次のセットの行（1行ごとに1つの原子）を読み取ります。string、atom、equal、world スタイルの変数は、next コマンドでは使用できません。これらの変数は単一の値しか格納しないためです。

next コマンド内のいずれかの変数に値がなくなると、フラグが設定され、これにより入力スクリプトは次に遭遇した [jump]() コマンドをスキップします。これにより、next コマンドを含むループが終了します。[variable]() コマンドで説明したように、値を使い切った変数は削除されます。これにより、その変数は後で再定義して使用できるようになります。file スタイルおよび atomfile スタイルの変数は、ファイルの終わりに達すると使い切られます。

next コマンドが index スタイルまたは loop スタイルの変数と共に使用されると、次の値がすべてのプロセッサに対してその変数に割り当てられます。next コマンドが file スタイルの変数と共に使用されると、そのファイルから次の行が読み取られ、文字列が変数に割り当てられます。next コマンドが atomfile スタイルの変数と共に使用されると、そのファイルから次の原子ごとの値のセットが読み取られ、変数に割り当てられます。next コマンドが universe スタイルまたは uloop スタイルの変数と共に使用されると、最初にコマンドを実行したプロセッサパーティションに次の値が割り当てられます。そのパーティション内のすべてのプロセッサは同じ値を割り当てられます。複数のプロセッサパーティションで LIGGGHTS(R)-PUBLIC を実行する方法は、このマニュアルのこのセクションで説明されています。universe スタイルおよび uloop スタイルの変数は、「tmp.lammps.variable」および「tmp.lammps.variable.lock」というファイルを使用してインクリメントされます。これらのファイルはそのような LIGGGHTS(R)-PUBLIC の実行中にディレクトリ内に表示されます。

次は、index スタイルの変数を使用して一連のシミュレーションを実行する例です。この入力スクリプトが in.polymer という名前の場合、run1 から run8 までのディレクトリのデータファイルを使用して、8 回のシミュレーションが実行されます。

```
variable d index run1 run2 run3 run4 run5 run6 run7 run8
shell cd $d
read_data data.polymer
run 10000
shell cd ..
clear
next d
jump in.polymer
```

もし変数「d」が universe スタイルであり、同じ in.polymer 入力スクリプトが 3 パーティションのプロセッサ上で実行される場合、最初の 3 回のシミュレーションがそれぞれのプロセッサセットで始まります。最初に終了したパーティションが変数「d」に 4 番目の値を割り当て、次のシミュレーションを実行します。このようにして、すべての 8 回のシミュレーションが終了するまで続きます。

また、ジャンプ（jump）および next コマンドはネストして使用することができ、これにより多階層のループを可能にします。例えば、次のスクリプトは 15 回のシミュレーションを二重ループで実行します。

```
variable i loop 3
  variable j loop 5
  clear
  ...
  read_data data.polymer.$i$j
  print Running simulation $i.$j
  run 10000
  next j
  jump in.script
next i
jump in.script
```

以下は、if および jump コマンドを使用して、条件が満たされたときに内側のループから抜け、外側のループを続ける例です。

```
label            loopa
variable    a loop 5
  label          loopb
  variable  b loop 5
  print          "A,B = $a,$b"
  run       10000
  if     $b > 2 then "jump in.script break"
  next           b
  jump           in.script loopb
label            break
variable    b delete
```

```
next     a
jump     in.script loopa
```

## 制限事項
none

## 関連コマンド
[jump](), [include](), [shell](), [variable](),

## デフォルト
none