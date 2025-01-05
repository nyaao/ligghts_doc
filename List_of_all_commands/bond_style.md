# bond_style command

## 構文
```
bond_style style args
```
style = one or hybrid or class2 or fene or fene/expand or harmonic or morse or nonlinear or quartic

none: 結合相互作用を無効にする
hybrid: 複数の結合スタイルを定義する
class2: Class2結合スタイル
fene: FENE（有限伸縮可能エラストマー）結合スタイル
fene/expand: 拡張FENE結合スタイル
harmonic: 調和的結合スタイル
morse: モース結合スタイル
nonlinear: 非線形結合スタイル
quartic: 四次結合スタイル

```
args = none for any style except hybrid
hybrid args = list of one or more styles
```

## 例
```
bond_style harmonic
bond_style fene
bond_style hybrid harmonic fene
```

## 説明
LIGGGHTS(R)-PUBLIC では、原子のペア間の結合相互作用を計算するための式を設定するために bond_style コマンドを使用します。LIGGGHTS(R)-PUBLIC における「結合」は、[pair_style]() コマンドで設定されるペア相互作用とは異なります。結合は特定の原子ペア間に定義され、シミュレーションの期間中（結合が破れる可能性がある場合を除き）持続します。結合された原子のリストは、データファイルまたはリスタートファイルから [read_data]() または [read_restart]() コマンドによって読み込まれます。

一方で、ペアポテンシャルは通常、カットオフ距離内のすべての原子ペア間で定義され、アクティブな相互作用のセットは時間とともに変化します。

異なる結合ポテンシャルを使用して結合を計算するハイブリッドモデルは、hybrid 結合スタイルを使用して設定できます。

結合スタイルに関連する係数は、データファイルやリスタートファイルで指定することも、bond_coeff コマンドを使用して指定することもできます。

すべての結合ポテンシャルはその係数データをバイナリリスタートファイルに保存するため、リスタートするシミュレーションで再度 [bond_style]() や [bond_coeff]() コマンドを入力する必要はありません。詳細については、[read_restart]() コマンドを参照してください。ただし、bond_style hybrid の場合、リスタートファイルにはサブスタイルのリストのみが保存されるため、結合係数は再度指定する必要があります。

![Warning]
結合スタイルとペアスタイルの両方が定義されている場合、special_bonds コマンドを使用して、結合された2つの原子間で本来存在するペア相互作用を無効にする（または重み付けする）必要があることがよくあります。

各結合スタイルでリストされた式において、r は結合にある2つの原子間の距離を示します。

以下は、LIGGGHTS(R)-PUBLIC で定義された結合スタイルのアルファベット順リストです。スタイルをクリックすると、そのスタイルが計算する式や、関連する [bond_coeff]() コマンドで指定された係数が表示されます。

また、LIGGGHTS(R)-PUBLIC 配布には、ユーザーが提出した追加の結合スタイルも含まれています。これらのスタイルのリストと、各スタイルへのリンクは、このページの「bond」セクションで確認できます。

- [bond_style none]() - 結合相互作用を無効にする
- [bond_style hybrid]() - 複数の結合スタイルを定義する
- [bond_style harmonic]() - 調和的結合

## 制限事項
結合スタイルは、結合を定義できる原子スタイルにのみ設定できます。

ほとんどの結合スタイルは MOLECULAR パッケージの一部であり、このパッケージが LIGGGHTS(R)-PUBLIC に組み込まれている場合にのみ有効になります。パッケージに関する詳細は、「LIGGGHTS(R)-PUBLIC のビルド」セクションを参照してください。個別の結合ポテンシャルに関するドキュメントページでは、それがどのパッケージに含まれているかも記載されています。

## 関連コマンド
[bond_coeff](), [delete_bonds]()

## デフォルト
bond_style none