#  compute erotate/multisphere command

## 構文
```
compute ID group-ID erotate/multisphere general_keyword general_values
```

- ID と group-ID は [compute]() コマンド内で説明されています。
- erotate/multisphere は、このcomputeコマンドのスタイル名です。
- general_keywords および general_values は [compute]() コマンド内で説明されています。

## 例
```
compute 1 all erotate/multisphere
```

## 説明
マルチスフィアボディの集合の回転運動エネルギーを計算する計算式を定義します。

各マルチスフィアボディの回転エネルギーは以下の式で計算されます：
1/2 I Wbody^2  
​
ここで、
I はマルチスフィアボディの慣性テンソル、
Wbodyはその角速度ベクトルです。
両方ともマルチスフィアボディの座標系で表されており、
I は対角化されています。

この計算は、自動的にマルチスフィアボディを定義する [fix multisphere]() コマンドに接続されます。この場合、compute コマンドで指定したグループは無視されます。計算には、fix multisphere コマンドで定義されたすべてのマルチスフィアボディの回転エネルギーが含まれます。

## 出力情報
この計算は、すべてのマルチスフィアボディの回転エネルギーを合計したグローバルスカラー値を計算します。この値は、計算結果としてグローバルスカラー値を入力として使用する任意のコマンドで利用できます。LIGGGHTS(R)-PUBLICの出力オプションの概要については、Section_howto 15を参照してください。

この計算で得られるスカラー値は「広範的」(extensive) なものです。スカラー値はエネルギー単位で表されます。

## 制限事項
none

## 関連コマンド
compute ke/multisphere

## デフォルト
none