# compute property/atom command

## 構文
```
compute ID group-ID property/atom general_keyword general_values input1 input2 ...
```

- ID、group-IDはcomputeコマンドで記載されています。
- property/atom = この計算コマンドのスタイル名
- general_keywords general_valuesはcomputeで記載されています
- input = 1つ以上の原子属性

可能な属性 = id、mol、type、mass、
x、y、z、xs、ys、zs、xu、yu、zu、ix、iy、iz、
vx、vy、vz、fx、fy、fz、
q、mux、muy、muz、mu、
radius、diameter、omegax、omegay、omegaz、
angmomx、angmomy、angmomz、
shapex、shapey、shapez、
quatw、quati、quatj、quatk、tqx、tqy、tqz、
end1x、end1y、end1z、end2x、end2y、end2z、
corner1x、corner1y、corner1z、
corner2x、corner2y、corner2z、
corner3x、corner3y、corner3z、
i_name、d_name

id = 原子ID  
mol = 分子ID  
type = 原子の種類  
mass = 原子の質量  
x, y, z = スケーリング前の原子座標  
xs, ys, zs = スケーリング後の原子座標  
xu, yu, zu = アンラップされた原子座標  
ix, iy, iz = 原子が含まれるボックスのイメージ  
vx, vy, vz = 原子の速度  
fx, fy, fz = 原子にかかる力  
q = 原子の電荷  
mux, muy, muz = 原子の双極子モーメントの方向  
mu = 原子の双極子モーメントの大きさ  
radius, diameter = 球状粒子の半径、直径  
omegax, omegay, omegaz = 球状粒子の角速度  
angmomx, angmomy, angmomz = 非球状粒子の角運動量  
shapex, shapey, shapez = 非球状粒子の3つの直径  
quatw, quati, quatj, quatk = 非球状またはボディ粒子のクォータニオン成分  
tqx, tqy, tqz = 有限サイズ粒子のトルク  
end12x, end12y, end12z = 線分の端点  
corner123x, corner123y, corner123z = 三角形のコーナーポイント  
i_name = カスタム整数ベクトルの名前  
d_name = カスタム浮動小数点ベクトルの名前  

## 例
```
compute 1 all property/atom xs vx fx mux
compute 2 all property/atom type
compute 1 all property/atom ix iy iz
```

## 説明
この計算は、グループ内の各原子に対して原子属性を単純に保存するものです。この計算は、他の[output commands]()（例えば、[compute reduce]()、[fix ave/atom]()、[fix ave/histo]()、[fix ave/spatial]()、および[atom-style variable]()コマンドなど）が計算結果を入力として使用できるようにするために便利です。

可能な属性のリストは、[dump custom]()コマンドで使用されるものと同じで、属性の意味が記述されており、特定の[atom style]()に対してのみ定義されている追加の量も含まれています。基本的には、このリストはLIGGGHTS(R)-PUBLICによって保存された任意の原子ごとの量にアクセスできるように、入力スクリプトに提供されます。

値は、以下に述べるように、原子ごとのベクトルまたは配列に保存されます。指定されたグループに含まれない原子や、その粒子に対して定義されていない量（例えば、粒子が楕円体でない場合のshapex）にはゼロが保存されます。

このコマンドを通じてのみアクセスできる追加の量（[dump custom]()コマンドでは直接アクセスできないもの）は、以下の通りです。

shapex、shapey、shapezは楕円体粒子に対して定義されており、各粒子の3次元形状を定義します。

quatw、quati、quatj、quatkは楕円体粒子およびボディ粒子に対して定義され、各粒子の向きを表す4次元ベクトルクォータニオンを保存します。クォータニオンベクトルの説明については、setコマンドを参照してください。

end1x、end1y、end1z、end2x、end2y、end2zは線分粒子に対して定義され、各線分の端点を定義します。

corner1x、corner1y、corner1z、corner2x、corner2y、corner2z、corner3x、corner3y、corner3zは三角形粒子に対して定義され、各三角形の角点を定義します。

i_nameとd_nameの属性は、fix property/atomコマンドを使用して各原子に追加されたカスタム整数および浮動小数点のプロパティを指します。そのコマンドが使用されると、各属性には特定の名前が付けられ、これがi_nameまたはd_nameの「名前」部分として指定されます。

## 出力情報
このコマンドは、入力値の数に応じて、原子ごとのベクトルまたは原子ごとの配列を計算します。単一の入力が指定されると、原子ごとのベクトルが生成されます。2つ以上の入力が指定されると、原子ごとの配列が生成され、その列数は入力の数と同じになります。ベクトルまたは配列は、計算からの原子ごとの値を入力として使用するコマンドによってアクセスできます。LIGGGHTS(R)-PUBLICの出力オプションの概要については、このセクションを参照してください。

ベクトルまたは配列の値は、対応する属性の単位で表されます。たとえば、vxの場合は速度単位、qの場合は電荷単位などです。

## 制限事項
none

## 関連コマンド
[dump custom](), [compute reduce](), [fix ave/atom](), [fix ave/spatial](), [fix property/atom]()

## デフォルト
none