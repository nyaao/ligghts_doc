# compute ke/atom command

## 構文
```
compute ID group-ID ke/atom general_keyword general_values
```
- ID、group-ID は compute コマンドで記録されています。
- ke/atom = この計算コマンドのスタイル名
- general_keywords general_values は compute で記録されています。

## 例
```
compute 1 all ke/atom
```

## 説明
各原子の並進運動エネルギーを計算する計算を定義します。

運動エネルギーは単純に 1/2 m v^2 で計算され、ここで m は質量、v は各原子の速度です。

指定された計算グループに含まれない原子の運動エネルギーの値は 0.0 になります。

## 出力情報
この計算は、各原子ごとのベクトルを計算します。このベクトルは、計算からの原子ごとの値を入力として使用する任意のコマンドによってアクセスできます。LIGGGHTS(R)-PUBLICの出力オプションの概要については、セクション_howto 15を参照してください。

各原子ごとのベクトルの値はエネルギー単位で表されます。

## 制限事項
none

## 関連コマンド
[dump custom]()

## デフォルト
none