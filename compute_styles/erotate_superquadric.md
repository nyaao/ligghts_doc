# compute erotate/superquadric command

## 構文
```
compute ID group-ID erotate/superquadric general_keyword general_values
```
- ID、group-IDは、computeコマンドでドキュメント化されています。
- erotate/superquadric = この計算コマンドのスタイル名
- 一般的なキーワードとその値は、computeでドキュメント化されています。

## 例
```
compute 1 all erotate/superquadric
```

## 説明
スーパーエリプソイド粒子のグループの回転運動エネルギーを計算する計算を定義します。これらのオプションの説明については、atom_styleおよびread_dataコマンドを参照してください。

すべての3タイプの粒子について、回転運動エネルギーは次のように計算されます：
$1/2 I w^2$  
ここで、Iは粒子の慣性テンソル、wはその角速度であり、必要に応じて角運動量から計算されます。

## 出力情報
この計算はグローバルスカラー（回転運動エネルギー）を計算します。この値は、計算から入力としてグローバルスカラー値を使用する任意のコマンドで使用できます。LIGGGHTS(R)-PUBLIC-SUPERQUADRICの出力オプションの概要については、セクション_howto 15を参照してください。

この計算によって得られるスカラー値は「拡張可能」です。このスカラー値はエネルギー単位で表されます。

## 制限事項
この計算は [atom_style superquadric]() を必要とします。

## 関連コマンド
[compute erotate/sphere]()

## デフォルト
none