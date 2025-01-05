# jump command

## 構文
```
jump file label
```

file = 新しい入力スクリプトのファイル名
label = ファイル内でジャンプ先のオプションラベル

## 例
```
jump newfile
jump in.run2 runloop
jump SELF runloop
```

## 説明
このコマンドは現在の入力スクリプトファイルを閉じ、指定された名前のファイルを開き、そのファイルからLIGGGHTS(R)-PUBLICのコマンドを読み取る操作を開始します。includeコマンドとは異なり、元のファイルには戻りません。ただし、複数のjumpコマンドを使用することで、ファイルからファイルへ、または元のファイルへとチェーンさせることが可能です。

ファイル名に「SELF」という語を指定した場合、現在の入力スクリプトが再び開かれ、再度読み取られます。

[!WARNING]SELFオプションは、現在の入力スクリプトが標準入力（stdin）を介して読み取られている場合には、動作が保証されません。例:
```
lmp_g++ < in.script
```

SELF オプションは C ライブラリの rewind() 呼び出しを使用するため、一部のシステムでは標準入力（stdin）に対してサポートされていない場合があります。この問題は、コマンドライン引数 -in を使用するか、または -var コマンドライン引数を使用してスクリプト名を変数として入力スクリプトに渡すことで回避できます。後者の場合、fname 変数を SELF の代わりに使用することができます。例:
```
lmp_g++ -in in.script
```
```
lmp_g++ -var fname n.script < in.script
```

jump コマンドの第2引数はオプションです。指定された場合、それはラベルとして扱われ、新しいファイルが（コマンドを実行することなく）そのラベルが見つかるまでスキャンされ、そこから先のコマンドが実行されます。この機能は、入力スクリプトの一部をループ処理するために使用できます。以下はその例です。

これらのコマンドは、各10,000ステップの10回のシミュレーションを実行し、file.1, file.2 などの名前を持つ10個のダンプファイルを作成します。[next]()コマンドは、10回の反復が終了した後にループを終了するために使用されます。「a」変数が10回目のインクリメントを受けると、次の jump コマンドがスキップされるようになります。
```
variable a loop 10
label loop
dump 1 all atom 100 file.$a
run 10000
undump 1
next a
jump in.lj loop
```


jump コマンドのファイル引数が変数である場合、jump コマンドを使用して、異なるプロセッサパーティションが異なる入力スクリプトを実行するようにすることができます。この例では、LIGGGHTS(R)-PUBLIC は40個のプロセッサで実行され、それらが10個のプロセッサからなる4つのパーティションに分割されています。この例の変数と jump コマンドを含む in.file を使用すると、各パーティションが異なるシミュレーションを実行するようになります。
```
mpirun -np 40 lmp_ibm -partition 4x10 -in in.file
```
```
variable f world script.1 script.2 script.3 script.4
jump $f
```

こちらは、if コマンドと jump コマンドを使用して、条件が満たされたときに内側のループを抜け、外側のループを続ける二重ループの例です。
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
指定されたラベルが含まれていないファイルにジャンプした場合、LIGGGHTS(R)-PUBLIC はファイルの最後に達し、終了します。

## 関連コマンド
[variable](), [include](), [label](), [next]()

## デフォルト
none
