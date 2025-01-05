# compute contact/atom command
# compute contact/atom/gran command

## 構文
```
compute ID group-ID contact/atom general_keywords general_values
compute ID group-ID contact/atom/gran general_keywords general_values
```

- ID、group-IDは[compute]()コマンドに記載されています。
- contact/atom または contact/atom/gran = このcomputeコマンドのスタイル名
- 一般的なキーワードと値は[compute]()に記載されています。

## 例
```
compute 1 all contact/atom
compute 1 all contact/atom/gran
```

## 説明
指定されたグループ内の各原子の接触数を計算する計算を定義します。

接触数は、有限サイズの球状粒子の場合、中心粒子に近い隣接原子の数として定義されます。具体的には、これらの原子の分離距離が2つの粒子の半径の合計とスキン距離の和以下である場合です。もしcontact/atom/granスタイルが選択される場合、接触は2つの粒子の重なり（距離 < 半径の和）または履歴でcontactflagが設定されている場合（例えば、結合接触による）として定義されます。

接触数の値は、指定された計算グループに含まれない原子には0.0となります。

この計算は、配位数を計算する3つの異なる方法の1つです。以下の表は、異なるオプションの概要を示します。

|style	contact| counted condition|	formula|
|---|---|---|
|compute contact/atom|	particles touch each other|	r < r_i + r_j|
|compute contact/atom/gran|	particles interact with each other|	f_ij > 0|
|compute coord/atom|	particles are in the vicinity of each other|	r < cutoff|

## 出力情報

この計算は、各原子に対してベクトルを計算します。その値は、計算からの原子ごとの値を入力として使用する任意のコマンドでアクセスできます。LIGGGHTS(R)-PUBLICの出力オプションの概要については、セクション15を参照してください。

各原子ごとのベクトルの値は、上記のように0.0以上の数値になります。

## 制限事項
この計算は、[atom_style sphere]() コマンドによって定義された半径を原子が格納していることを要求します。

この計算は、すべての球状粒子を個別に扱うため、複数の球体を扱うシミュレーションでは正しい結果を得られない可能性があります。

## 関連コマンド
[compute coord/atom]()

## デフォルト
none