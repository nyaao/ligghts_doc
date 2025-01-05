# orient command

## 構文
```
orient dim i j k
```
dim = x または y または z
i, j, k = 格子がボックス方向 dim に沿った向き

## 例
```
orient x 1 1 0
orient y -1 1 0
orient z 0 0 1
```

## 説明
シミュレーションボックスの方向 x、y、または z に沿った立方格子の向きを指定します。これらの3つの基底ベクトルは、[create_atoms]() コマンドが原子の格子を生成する際に使用されます。

3つの基底ベクトル B1、B2、B3 は互いに直交しており、右手系を形成する必要があります。つまり、B1 × B2 は B3 の方向にあるべきです。

基底ベクトルは、最小の整数を使用した不可逆な形で指定するべきですが、LIGGGHTS(R)-PUBLIC ではこれをチェックしません。

## 制限事項
none

## 関連コマンド
[origin](), [create_atoms]()

## デフォルト
```
orient x 1 0 0
orient y 0 1 0
orient z 0 0 1
```