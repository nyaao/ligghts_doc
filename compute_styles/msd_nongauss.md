# compute msd/nongauss command

## 構文
```
compute ID group-ID msd/nongauss general_keyword general_values keyword values ...
```
- ID、group-IDはcomputeコマンドで文書化されています。
- msd/nongauss = この計算コマンドのスタイル名
- general_keywords general_valuesはcomputeで文書化されています。
- ゼロ個以上のキーワード/値のペアを追加することができます。
- キーワード = com  
comの値 = yes または no

## 例
```
compute 1 all msd/nongauss
compute 1 upper msd/nongauss com yes
```

## 説明
次の計算を定義します。この計算は、原子群の平均二乗変位（MSD）と非ガウスパラメータ（NGP）を計算し、原子が周期境界を越える効果も考慮に入れます。

この計算では、3つの量からなるベクトルが計算されます。ベクトルの最初の要素は、原子のdx、dy、dzの変位の合計平方、すなわちdrsquared = (dxdx + dydy + dzdz)です。2番目の要素は、これらの変位の4乗であり、drfourth = (dxdx + dydy + dzdz)(dxdx + dydy + dzdz)として、グループ内の原子に対して合計し、平均した値です。3番目の要素は、非ガウス拡散パラメータNGPであり、NGP = 3drfourth/(5drsquared*drsquared)となります。

$$
NGP(t) = 3 \langle \left( r(t) - r(0)\right)^4 \rangle / \left( 5  \langle\left( r(t) - r(0) \right)^2 \rangle^2 \right)-1
$$

NGPは動的異質性の研究で一般的に使用される量です。その最小理論値（-0.4）は、すべての原子が同じ変位の大きさを持つ場合に発生します。NGP=0はブラウン運動の拡散に対応し、NGP > 0は一部の移動する原子が他の原子よりも速く移動する場合に発生します。

もしcomオプションが「yes」に設定されている場合、原子群の質量中心のドリフト効果が補正され、各原子の変位が計算される前にその影響が除去されます。

この計算に適用されるその他の重要な注意点については、[compute msd]()ドキュメントページを参照してください。

## 出力情報
この計算は、長さ3のグローバルベクトルを計算します。このベクトルは、インデックス1-3を使用して、計算からグローバルベクトル値を入力として使用する任意のコマンドからアクセスできます。LIGGGHTS(R)-PUBLICの出力オプションの概要については、このセクションを参照してください。

ベクトルの値は「集中的」です。最初のベクトル値は距離^2の[units]()で、2番目は距離^4の単位で、3番目は無次元です。

## 制限事項
この計算はMISCパッケージの一部です。このパッケージは、LIGGGHTS(R)-PUBLICがそのパッケージでビルドされている場合のみ有効です。詳細については、「LIGGGHTS(R)-PUBLICの作成」セクションを参照してください。

## 関連コマンド
[compute msd]()

## デフォルト
オプションのデフォルトは com = no です。