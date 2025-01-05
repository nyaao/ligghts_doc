# compute erotate command

## 構文
```
compute ID group-ID erotate general_keywords general_values
```

- ID、group-IDは[compute]()コマンドで文書化されています。
- erotate = このcomputeコマンドのスタイル名
- 一般的なキーワードとその値は[compute]()コマンドで文書化されています。

## 例
```
compute 1 all erotate
```

## 説明
全ての粒子の回転運動エネルギーを計算する計算を定義します。この計算は、球形、スーパー楕円体、及び多球粒子の回転運動エネルギーを合計します。

これらのエネルギーがどのように計算されるかの詳細は、それぞれの子計算に記載されています。

## 出力情報
この計算は、グローバルスカラー（回転運動エネルギー）を計算します。この値は、グローバルスカラー値を入力として使用する任意のコマンドで使用できます。LIGGGHTS(R)-PUBLICの出力オプションの概要については、セクション15を参照してください。

この計算によって得られるスカラー値は「広がりのある」値です。スカラー値はエネルギー[units]()で表されます。

## 制限事項
それらは子計算のものです。

## 関連コマンド
[compute erotate/sphere](), [compute erotate/mulitsphere](), [compute erotate/superquadric]()

## デフォルト
none