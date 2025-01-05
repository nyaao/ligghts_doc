# write_dump command 

## 構文
```
write_dump group-ID style file dump-args modify dump_modify-args
```
group-ID = ダンプ対象となる原子群のID
style = サポートされている[dump_style]()のいずれか
file = ダンプ情報を出力するファイルの名前
dump-args = 特定の[dump_style]()に必要な追加の引数
modify = このキーワード以降のすべての引数は [dump_modify]() に渡されます（オプション）
dump-modify-args = [dump_modify]() の引数（オプション）

## 例
```
write_dump all atom dump.atom
write_dump subgroup atom dump.run.bin
write_dump all custom dump.myforce.* id type x y vx fx
write_dump flow custom dump.%.myforce id type c_myF[3] v_ke modify sort id
write_dump all xyz system.xyz modify sort id elements O H
write_dump all image snap*.jpg type type size 960 960 modify backcolor white
write_dump all image snap*.jpg element element &
   bond atom 0.3 shiny 0.1 ssao yes 6345 0.2 size 1600 1600  &
   modify backcolor white element C C O H N C C C O H H S O H
```

## 説明
システムの現在の状態に関する原子の量のスナップショットを1回限り、1つまたは複数のファイルにダンプします。これは一度限りの即時操作であり、ランニングシミュレーション中に定期的にスナップショットを出力するためにダンプスタイルを設定する[dump]()コマンドとは異なります。

このコマンドの構文は、[dump]()コマンドと[dump_modify]()コマンドが結合されたような形でほぼ同じですが、以下の例外があります：ダンプIDやダンプ頻度は必要ありませんし、キーワード「modify」が追加されます。後者は、複数のスナップショットと同様に、単一のスナップショットに対して[dump_modify]()の全範囲オプションを指定できるようにするためです。「modify」キーワードは、通常ダンプコマンドに渡される引数と、dump_modifyに渡される引数を区別します。両方ともオプションの引数をサポートしており、LIGGGHTS(R)-PUBLICは2つの引数セットを明確に区別できる必要があります。

指定したファイル名に[dump]()コマンドでサポートされているワイルドカード文字「\*」や「%」が使用されている場合、それらは新しいファイル名を作成するために同じ方法で機能します。通常、[dump image]()ファイルはタイムステップのために「\*」文字を含むファイル名が必要ですが、write_dumpコマンドではその必要はなく、ワイルドカード「\*」文字は必要ありません。

## 制限事項

[dump]()コマンドおよび[dump_modify]()コマンドに対するすべての制限は、このコマンドにも適用されますが、上記のように[dump_image]()ファイル名にはワイルドカード「*」文字が必要ないという例外があります。

ダンプは通常、[run]()中またはエネルギー最小化中に書き込まれるため、このコマンドを使用する前にシミュレーションが実行準備が整っている必要があります。同様に、ダンプが計算、修正、または変数から情報を必要とする場合、その情報は現在のタイムステップ（例えば、前回の実行によって）に計算されている必要があります。さもなければ、LIGGGHTS(R)-PUBLICはエラーメッセージを生成します。

例えば、このコマンドで実行前に各原子のエネルギーをダンプすることはできません。なぜなら、エネルギーや力はまだ計算されていないからです。このトピックに関する詳細は、[variable]()のドキュメントページの「変数の精度」セクションをご参照ください。

## 関連コマンド
[dump](), [dump image](), [dump_modify]()

## デフォルト
デフォルト値は、[dump](), [dump image](), [dump_modify]()の各コマンドのドキュメントページに記載されています。
