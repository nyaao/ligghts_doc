# bond_style harmonic_command

## 構文
```
bond_style harmonic
```


## 例
```
bond_style harmonic
bond_coeff 5 80.0 1.2
```


## 説明
「harmonic bond スタイルは以下のポテンシャルを使用します：

$$
E = K \left( r - r_0 \right)^2
$$
ここで、r0 は平衡結合距離を示します。なお、通常の 1/2 の係数は K に含まれています。

各結合タイプごとに、以下の係数を bond_coeff コマンド、または read_data や read_restart コマンドで読み込むデータファイルや再起動ファイル内で定義する必要があります：

- K (エネルギー/距離²)
- r0 (距離)」

## 制限事項
「この結合スタイルは、LIGGGHTS(R)-PUBLIC が MOLECULAR パッケージとともにビルドされている場合にのみ使用できます（デフォルトではビルドされています）。パッケージに関する詳細は、LIGGGHTS(R)-PUBLIC の作成セクションを参照してください。」

## 関連コマンド
[bond_coeff](), [delete_bonds]()

## デフォルト
none