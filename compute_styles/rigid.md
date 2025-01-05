# compute rigid command
# compute multisphere command
# compute multisphere/single command

## 構文
```
compute ID group-ID multisphere general_keyword general_values (or multisphere) property property_name
compute ID group-ID multisphere/single id id_ms general_keyword general_values (or multisphere) property property_name
compute ID group-ID rigid general_keyword general_values (or multisphere) property property_name
```


- ID, group-IDはcomputeコマンドに記載されています。
- id = multisphere/singleスタイルの必須キーワード
- id_ms = multisphere/singleスタイルに適用される多球体のID
- property = 必須キーワード
- general_keywords general_valuesはcomputeコマンドに記載されています。
- property_name = xcm、vcm、fcm、torque、quat、angmom、omega、density、type、id、masstotal、inertia、ex_space、ey_space、ez_space

xcm = 体の位置（重心に基づく）（3つの値）  
vcm = 体の速度（重心に基づく）（3つの値）  
fcm = 体の力（重心に基づく）（3つの値）  
torque = 体のトルク（重心に基づく）（3つの値）  
quat = 体のクォータニオン（重心に基づく）（4つの値）  
angmom = 体の角運動量（重心に基づく）（3つの値）  
omega = 体の角速度（重心に基づく）（3つの値）  
density = 体の密度（1つの値）  
atomtype = 剛体の原子タイプ（材料タイプ）（1つの値）  
clumptype = fix paticleteplate/multisphereで定義された多球体タイプ（1つの値）  
id_multisphere = 体のID（1つの値）  
masstotal = 体の質量（1つの値）  
inertia = 体の慣性（重心に基づき、ex_space, ey_space, ez_space周り）（3つの値）  
ex_space, ey_space, ez_space = 体の固有系（重心に基づく）（それぞれ3つの値）  

## 例
```
compute xcm all multisphere property xcm
```

## 説明
個々の多球体（クランプ）物体のプロパティを計算するcomputeコマンドを定義します。この多球体は、[fix particletemplate/multisphere]()を使用してシミュレーション内で定義されます。

multisphereスタイルでは、システム内のすべての多球体物体のデータが生成されます。このコマンドによって格納されるローカルデータは、プロセス上で所有されているすべての物体をループして生成されます。

multisphere/singleスタイルでは、指定されたIDの多球体物体のデータのみが生成されます。

[!WARNING]このコマンドではgroup-IDは無視されます。というのも、グループデータは原子ベースであり、クランプベースではないためです。

## 出力情報
multisphereスタイルの場合、このcomputeコマンドはローカルベクトルまたはローカル配列を計算します（データの長さに応じて、上記を参照）。ベクトルまたは配列は、computeからのローカル値を入力として使用するコマンドからアクセスできます。このセクションでは、LIGGGHTS(R)-PUBLICの出力オプションの概要を確認できます。

同様に、multisphere/singleスタイルの場合、このcomputeコマンドは、データの長さに応じてグローバルスカラーまたはベクトルを計算します（上記を参照）。

## 制限事項
粒子間ペアスタイルと一緒にのみ使用できます。粒子と壁の接触データにアクセスするには、メッシュ壁のみを使用できます。

## 関連コマンド
[dump local](), [compute property/local]()

## デフォルト
none