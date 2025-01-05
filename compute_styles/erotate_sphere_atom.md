# compute erotate/sphere/atom command

## 構文
```
compute ID group-ID erotate/sphere/atom general_keyword general_values
```

- IDおよびgroup-IDについては、[compute]()コマンドのドキュメントに記載されています。
- erotate/sphere/atomは、このcomputeコマンドのスタイル名です。
- general_keywordsおよびgeneral_valuesについては、[compute]コマンドのドキュメントに記載されています。

## 例
```
compute 1 all erotate/sphere/atom
```

## 説明
指定したグループ内の各粒子の回転運動エネルギーを計算する演算を定義します。

回転エネルギーは、以下の式で計算されます:
$1/2 I w^2$
ここで、I は球体の慣性モーメント、w は粒子の角速度です。

[!WARNING]2次元モデルの場合、粒子は円盤ではなく球体として扱われるため、慣性モーメントは3次元の場合と同じになります。

回転運動エネルギーの値は、次のいずれかの場合に0.0となります:
指定された計算グループに属していない原子
半径が0.0の点粒子

## 出力情報
この計算は、各原子ごとに値を持つベクトル（per-atom vector）を生成します。この値は、計算結果を入力として利用する任意のコマンドからアクセス可能です。LIGGGHTS(R)-PUBLICの出力オプションに関する概要については、Section_howto 15を参照してください。

このベクトルの各原子に対応する値は、エネルギー[units]()で表されます。

## 制限事項
none

## 関連コマンド
[dump custom]()

## デフォルト
none