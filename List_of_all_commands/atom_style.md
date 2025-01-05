# atom_style command

## 構文
```
atom_style style args
```
style = bond（結合）または charge（電荷）または ellipsoid（楕円体）または full（フル）または line（線）または molecular（分子）または tri（三角形）または hybrid（ハイブリッド）または sphere（球）または granular（粒状）または bond/gran（結合/粒状）または superquadric（スーパー楕円体）または convexhull（凸包）または sph（球）

```
args = none for any style except body and hybrid
body args = bstyle bstyle-args
  bstyle = style of body particles
  bstyle-args = additional arguments specific to the bstyle
                see the body doc page for details
hybrid args = list of one or more sub-styles, each with their args
```

 ## 例
 ```
 args = none for any style except body and hybrid
body args = bstyle bstyle-args
  bstyle = style of body particles
  bstyle-args = additional arguments specific to the bstyle
                see the body doc page for details
hybrid args = list of one or more sub-styles, each with their args
 ```

 ## 説明
シミュレーションで使用する原子のスタイルを定義します。これにより、原子に関連付けられる属性が決まります。このコマンドは、[read_data]()、[read_restart]()、または [create_box]() コマンドでシミュレーションをセットアップする前に使用しなければなりません。

一度スタイルが設定されると変更できないため、すべての属性を網羅するような一般的なスタイルを選択する必要があります。例えば、bond スタイルを使用すると、角度項などは後からモデルに追加することができません。ただし、必要以上に一般的なスタイルを使うことは許容されており、その場合、少し非効率的になることがあります。

スタイルの選択は、各原子が保持する量、プロセッサ間で計算に必要な力を計算できるように通信される量、そして[read_data]() コマンドで読み込まれるデータファイルに記載される量に影響を与えます。

各スタイルの追加属性と、それらが使用される典型的な物理システムの種類は以下の通りです。すべてのスタイルは、座標、速度、原子ID、およびタイプを保存します。これらのさまざまな量を設定する方法については、[read_data]()、[create_atoms]()、および [set]() コマンドの説明を参照してください。

||||
|---|---|---|
|bond|	bonds|	bead-spring polymers|
|bond/gran|	number of bonds and bond information|	granular bond models|
|charge|	charge|	atomic system with charges|
|convexhull|	mass, angular velocity, quaternion|	granular models|
|ellipsoid|	shape, quaternion, angular momentum|	aspherical particles|
|line|	end points, angular velocity|	rigid bodies|
|molecular|	bonds, angles, dihedrals, impropers|	uncharged molecules|
|sph|	q(pressure), density|	SPH particles|
|sphere or granular|	diameter, mass, angular velocity|	granular models|
|superquadric|	semi-axes, blockiness parameters, mass, angular velocity, quaternion|	granular models|
|tri|	corner points, angular momentum|	rigid bodies AWPMD|

![warning]  
一部の属性（例えば分子IDなど）を、元々その属性を持っていない原子スタイルに追加することは、fix property/atom コマンドを使用することで可能です。このコマンドを使用すると、整数または浮動小数点数の追加のカスタム属性を原子に追加することもできます。カスタム値の初期化、アクセス、および出力方法の詳細や、この機能が役立つ例については、fix property/atom のドキュメントページを参照してください。


すべてのスタイルは、mass コマンドを使用して、粒子の質量をタイプ別に割り当てますが、sphere や granular スタイルでは、個々の粒子に対して質量が割り当てられます。

sphere スタイルでは、粒子は球体であり、各粒子は個別の直径と質量を保持します。もし直径が 0.0 より大きければ、その粒子は有限のサイズを持つ球体です。直径が 0.0 の場合、それは点粒子です。これは通常、粒状物質モデルで使用されます。sphere の代わりに granular キーワードも使用可能です。

bond/gran スタイルでは、各原子に対して粒状の結合の数が保存され、その結合に関連する情報（各結合のタイプ、結合パートナー原子のID、およびいわゆる結合履歴）も保存されます。結合履歴は粒状相互作用の接触履歴に似ており、結合の内部状態を保存します。この内部状態に何が保存されるかは、使用される粒状結合スタイルによって定義されます。これには2つのパラメータがあります。1つは結合のタイプの数、もう1つは各原子が持つことのできる最大結合数です。各結合タイプに対して、これらのパラメータは bond_coeff コマンドを使用して指定する必要があります（例については、ここを参照してください）。なお、bond/gran は実験的なコードであり、使用しているLIGGGHTSのリリースには含まれていない可能性があります。以下にその構文の例を示します。

```
atom_style bond/gran n_bondtypes 1 bonds_per_atom 6
```

ellipsoid スタイルでは、粒子は楕円体であり、各粒子は、有限サイズの楕円体か点粒子かを示すフラグを保持します。楕円体の場合、楕円体の3つの直径を持つ形状ベクトルと、その向きを示す4次元のクォータニオンベクトルも保存されます。

line スタイルでは、粒子は理想化された線分であり、各粒子は個別の質量、長さ、及び向きを（すなわち線分の端点）保持します。

tri スタイルでは、粒子は平面三角形であり、各粒子は個別の質量、サイズ、及び向きを（すなわち三角形の角の点）保持します。

ション内の一部の原子が特定のスタイルで定義されるすべてのプロパティを持っていない場合は、必要なすべてのプロパティを定義する最も単純なスタイルを使用します。例えば、シミュレーション内の一部の原子が電荷を持っているが他の原子は持っていない場合は、charge スタイルを使用します。もし一部の原子が結合を持っているが他の原子は持っていない場合は、bond スタイルを使用します。

hybrid スタイルが必要なのは、すべての原子の必要なプロパティを定義する単一のスタイルがない場合のみです。例えば、トルクによって回転する双極子粒子を使用したい場合は、「atom_style hybrid sphere dipole」を使用する必要があります。ハイブリッドスタイルが使用されると、原子は個々のスタイルによって示されるすべての量の和を保持し、通信します。

LIGGGHTS(R)-PUBLICは、新しい原子スタイルや新しいボディスタイルを追加することで拡張可能です。[このセクション](9. Modifying & extending LIGGGHTS(R)-PUBLIC)を参照してください。

## 制限事項
このコマンドは、シミュレーションボックスが read_data または create_box コマンドによって定義された後には使用できません。

superquadric スタイルは、まだPUBLICバージョンでは利用できません。

convexhull スタイルも、まだPUBLICバージョンでは利用できません。

bond と molecular スタイルは MOLECULAR パッケージの一部です。

line と tri スタイルは ASPHERE パッケージの一部です。これらのスタイルは、LIGGGHTS(R)-PUBLICがそのパッケージでビルドされている場合にのみ有効です。詳細は Making LIGGGHTS(R)-PUBLIC セクションを参照してください。

## 関連コマンド
[read_data](), [pair_style]()