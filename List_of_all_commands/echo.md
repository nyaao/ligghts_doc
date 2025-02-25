# echo command

## 構文
```
echo style
```
`style` キーワードは、LIGGGHTS の `dump` コマンドの出力のフォーマットや表示方法を定義します。以下のいずれかの値を取ります：

- **none**: 特にスタイルを指定しない。出力に追加のフォーマットは適用されません。
- **screen**: 出力を画面に表示します。通常、シミュレーションの結果をリアルタイムで表示したい場合に使用します。
- **log**: 出力をログファイルに記録します。後でデータを分析したい場合に役立ちます。
- **both**: 出力を画面とログファイルの両方に同時に表示します。リアルタイムでフィードバックを得ながら、データをログに保存するのに便利です。

これらのスタイルは、シミュレーションデータをどのように管理・表示したいかによって使い分けます。

## 例
```
echo both
echo log
```

## 説明
このコマンドは、LIGGGHTS(R)-PUBLICが入力スクリプトの各コマンドを読み込んで処理する際に、画面やログファイルにエコー（表示）するかどうかを決定します。入力スクリプトにエラーがある場合、エコーされた出力を見ることで最後に処理されたコマンドを確認するのに役立ちます。

[command-line switch]() -echo をこのコマンドの代わりに使用することもできます。

## 制限事項
none

## 関連コマンド
none

## デフォルト
```
echo log
```