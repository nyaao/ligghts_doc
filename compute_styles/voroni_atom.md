# compute voronoi/atom command

## 構文
```
compute ID group-ID voronoi/atom general_keyword general_values keyword arg ...
```
- IDおよびgroup-IDは、computeコマンドで説明されています。
- voronoi/atomは、このcomputeコマンドのスタイル名です。
- general_keywordsおよびgeneral_valuesは、computeコマンドで説明されています。
- 0個以上のキーワード/値のペアを追加することができます。

キーワードの説明:  
only_group: 引数なし  
surface: 引数 = sgroup-ID  
sgroup-ID: group-IDとsgroup-IDの間の分割面を計算します。このキーワードを使用すると、compute出力に3列目が追加されます。  
radius: 引数 = v_r  
v_r: ポリ分散Voronoi分割用の半径（原子スタイル変数）。  
edge_histo: 引数 = maxedge  
maxedge: ヒストグラムで考慮されるVoronoiセルの最大エッジ数。  
edge_threshold: 引数 = minlength  
minlength: 計算に含めるエッジの最小長さ。  
face_threshold: 引数 = minarea  
minarea: 計算に含める面の最小面積。

## 例
```
compute 1 all voronoi/atom
compute 2 precipitate voronoi/atom surface matrix
compute 3b precipitate voronoi/atom radius v_r
compute 4 solute voronoi/atom only_group
```

## 説明
シミュレーションボックス内の原子のVoronoi分割を計算する処理を定義します。この分割は、シミュレーション内のすべての原子を対象に計算されますが、グループ内の原子に対してのみ非ゼロの値が格納されます。

デフォルトでは、このcomputeによって原子ごとに2つの量が計算されます。1つ目は、各原子の周囲のVoronoiセルの体積です。任意の点がその原子のVoronoiセル内にある場合、それは他のどの原子よりもその原子に近いことを意味します。2つ目は、Voronoiセルの面の数であり、これはそのセルの中心にある原子の最近接隣接原子の数でもあります。

only_groupキーワードが指定されている場合、分割はcomputeグループ内に含まれる原子に対してのみ実行されます。これは、分割を評価する前にグループに含まれていないすべての原子を削除することと同等です。

surfaceキーワードが指定されている場合、各原子に対して3つ目の量が計算されます：指定された原子のVoronoiセルの表面積です。surfaceはグループIDを引数として受け取ります。「all」以外のグループが指定された場合、指定されたグループ内の隣接原子に面しているVoronoiセルの面のみが表面積として計算されます。

上記の例（マトリックス内に埋め込まれた析出物）では、析出物の表面にある原子のみが非ゼロの表面積を持ち、Voronoiセルの外側に向いた面のみが計算に含まれます（析出物の外殻）。析出物の総表面積は、c_2[3]に対して「reduce sum」計算を実行することで取得できます。

radiusキーワードが原子スタイル変数を引数として指定された場合、ポリ分散Voronoi分割が実行されます。半径変数の例としては以下が挙げられます：

```
variable r1 atom (type==1)*0.1+(type==2)*0.4
compute radius all property/atom radius
variable r2 atom c_radius
```

ここで、v_r1はタイプ1の原子に対して0.1単位、タイプ2の原子に対して0.4単位の1原子ごとの半径を指定し、v_r2は粒状モデルのためにatom_style sphereで存在する半径プロパティにアクセスします。

edge_histoキーワードは、計算対象のグループ内のVoronoiセルの面におけるエッジ数のヒストグラムの生成を有効にします。このキーワードの引数maxedgeは、サンプル内で予想される最大のVoronoiセル面のエッジ数です。このキーワードは、maxedge+1個のエントリを持つグローバルベクトルの生成を追加します。ベクトルの最後のエントリには、maxedge個のエッジを超える面の数が含まれます。最小のエッジ数を持つポリゴンは三角形であるため、ベクトルのエントリ1および2は常にゼロになります。

edge_thresholdおよびface_thresholdキーワードは、指定された最小長さ未満のエッジおよび最小面積未満の面を抑制するために使用されます。非常に短いエッジや非常に小さい面は、Voronoi分割のアーティファクトとして発生することがあります。これらのキーワードは、隣接原子の数やエッジのヒストグラム出力に影響を与えます。

Voronoi計算は、UCバークレーおよびLBLのChris Rycroftによって書かれた、自由に利用可能なVoro++パッケージによって実行されます。このパッケージは、LIGGGHTS(R)-PUBLICをこのcomputeで使用するためにビルドする際に、システムにインストールされている必要があります。Voro++ソフトウェアの取得とインストールに関する指示は、src/VORONOI/READMEファイルに記載されています。

[!WARNING]Voronoi体積の計算は、各プロセッサーが所有する原子に対して行われ、プロセッサーに保存されたゴースト原子の影響を含みます。これは、所有する原子のVoronoiセルがゴースト原子のカットオフ距離を超える原子に影響されないと仮定しています。これは通常、液体や固体システムにおいては良い仮定ですが、低密度システムではVoronoi体積の過小評価を招く可能性があります。デフォルトでは、各プロセッサーが保存するゴースト原子のセットは、[pair_style]()相互作用で使用されるカットオフによって決定されます。カットオフは、[communicate cutoff]()コマンドを使用して明示的に設定できます。

[!WARNING]Voro++パッケージは、計算を3次元で実行します。これは、2次元のLIGGGHTS(R)-PUBLICシミュレーションでも正常に動作し、実際にVoronoi「面積」を計算することができます。ただし、ボックスのz次元が原子間の分離と比較しておおよそ同じか、それより小さい場合に限ります。2次元LIGGGHTS(R)-PUBLICモデルでのz方向のボックス寸法の典型的な値は-0.5から0.5で、ほとんどの[units]() systemsの基準を満たします。2次元シミュレーションを行う際には、[create_box]()または[read_data]()コマンドを使用してシミュレーションボックスのz方向の範囲を定義することに注意してください。

## その他情報
このcomputeは、2列の1原子ごとの配列を計算します。最初の列はVoronoi体積で、2番目の列は上記で説明された隣接原子の数です。これらの値は、computeからの1原子ごとの値を入力として使用する任意のコマンドからアクセスできます。LIGGGHTS(R)-PUBLICの出力オプションの概要については、Section_howto 15を参照してください。

Voronoiセルの体積は、距離の3乗の[units]()で表されます。

## 制限事項
このcomputeは、VORONOIパッケージの一部です。このパッケージが有効であるのは、LIGGGHTS(R)-PUBLICがそのパッケージでビルドされている場合のみです。詳細については、「Making LIGGGHTS(R)-PUBLIC」セクションを参照してください。

## 関連コマンド
[dump custom]()

## デフォルト
none