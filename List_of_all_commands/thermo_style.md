# thermo_style command

## 構文
```
thermo_style style args
```
- style = one または multi または custom
- args = 特定のスタイルの引数リスト

one の引数：
引数なし

multi の引数：
引数なし

custom の引数：  
以下の属性のリスト：  
step = タイムステップ  
elapsed = この実行開始からのタイムステップ数  
elaplong = 複数回の実行の最初からのタイムステップ数  
dt = タイムステップの大きさ  
time = シミュレーション時間  
cpu = 経過したCPU時間（秒）  
tpcpu = 1 CPU秒あたりの時間  
spcpu = 1 CPU秒あたりのタイムステップ数  
cpuremain = 実行の残りCPU時間の推定値  
part = 現在のパーティション番号（0からNpartition-1）  
cu = 1 CPU秒あたりのタイムステップ数  
atoms = 原子数  
vol = 体積  
lx, ly, lz = x, y, z方向のボックスの長さ  
xlo, xhi, ylo, yhi, zlo, zhi = ボックスの境界  
xy, xz, yz = 非直交（トリクリニック）シミュレーションボックスの傾き  
xlat, ylat, zlat = lattice コマンドで計算された格子間隔  
pxx, pyy, pzz, pxy, pxz, pyz = 圧力テンソルの6つの成分  
fmax = すべての原子の中で、どの次元においても最大の力の成分  
fnorm = すべての原子の力ベクトルの長さ  
cella, cellb, cellc = 周期的セルの格子定数 a, b, c  
cellalpha, cellbeta, cellgamma = 周期的セルの角度 α, β, γ  
c_ID = 計算により得られたグローバルスカラー値（IDによる）  
c_ID[I] = 計算により得られたグローバルベクトルのI番目の成分（IDによる）  
c_ID[I][J] = 計算により得られたグローバル配列のI,J番目の成分（IDによる）  
f_ID = フィックスによって計算されたグローバルスカラー値（IDによる）  
f_ID[I] = フィックスによって計算されたグローバルベクトルのI番目の成分（IDによる）  
f_ID[I][J] = フィックスによって計算されたグローバル配列のI,J番目の成分（IDによる）  
v_name = 名前付きの等式型変数によって計算されたスカラー値  

## 例
```
thermo_style one
thermo_style custom step v_abc
```

## 説明
熱力学データを画面とログファイルに出力するスタイルと内容を設定します。

スタイル one：熱力学情報の1行要約を出力します。これは「thermo_style custom step atoms ke cpu」と同等で、行には数値のみが含まれます。

スタイル multi：熱力学情報の複数行リストを出力します。これは「thermo_style custom step atoms ke cpu」と同等で、リストには数値と各量に対応する文字列IDが含まれます。

スタイル custom：最も一般的な設定で、上記のキーワードのうち、各熱力学タイムステップで出力したいものを指定できます。c_ID、f_ID、v_nameのキーワードは、入力スクリプト内で他の場所で定義された計算（[compute]()）、修正（[fix]()）、または等式型変数（equal-style [variable]()）を参照しています。これらは新しいスタイルとしてLIGGGHTS(R)-PUBLICに追加することもできます（ドキュメントのSection_modifyセクションを参照）。したがって、customスタイルは、シミュレーションの進行に伴って実質的に任意の量を出力するための柔軟な手段を提供します。

customスタイル以外のすべてのスタイルでは、シミュレーションボックスの体積がシミュレーション中に変化する場合、その出力リストにvolが追加されます。

さまざまなキーワードによって出力される値は瞬時の値であり、現在のタイムステップで計算されます。前のタイムステップからの値を含む時間平均値は、f_IDキーワードを使用して、時間平均を行う修正（例：fix ave/timeコマンド）にアクセスすることによって出力できます。

thermo_modifyコマンドで呼び出されたオプションは、出力の1行または複数行のフォーマット、熱力学的出力の正規化（系の原子数に比例する広範な量に対する総量または原子ごとの値）、および各印刷値の数値精度を設定するために使用できます。

[!WARNING]「thermo_style」コマンドを使用すると、すべての熱力学設定がデフォルト値にリセットされます。これには、以前に「[thermo_modify]()」コマンドで設定された値も含まれます。したがって、入力スクリプトで「thermo_style」コマンドを指定する場合は、その後に「thermo_modify」コマンドを使用する必要があります。

step、elapsed、およびelaplongキーワードはタイムステップのカウントに関連します。stepは現在のタイムステップ、または最小化を実行している場合の反復回数です。elapsedはこの実行の開始から経過したタイムステップ数です。elaplongは、複数の実行の最初の実行開始から経過したタイムステップ数です。最初の実行時間を追跡するシリーズの実行を開始する方法については、runのstartおよびstopキーワードを参照してください。これらのキーワードが使用されない場合、elapsedとelaplongは同じ値になります。

dtキーワードは、time[unit]()における、現在のタイムステップサイズです。timeキーワードは現在の経過シミュレーション時間で、タイムステップサイズが変更されていない場合、またはタイムステップがリセットされていない場合は単純に (step * dt) となります。もしタイムステップが変更された場合（例えば、[fix dt/reset]()を使用した場合）やタイムステップがリセットされた場合（例えば、reset_timestepコマンドを使用した場合）、シミュレーション時間は現在の時点までの累積値となります。

cpuキーワードはこの実行の開始から経過したCPU秒数です。tpcpuおよびspcpuキーワードはシミュレーションが現在どれくらい速く実行されているかを示します。tpcpuキーワードは1CPU秒あたりのシミュレーション時間で、シミュレーション時間はtime [unit]()で示されます。例えば、metal単位の場合、tpcpuの値はピコ秒毎CPU秒となります。spcpuキーワードは1CPU秒あたりのタイムステップ数です。これらの量はどちらもオン・ザ・フライのメトリックで、最後にそれらが呼び出された時点を基準に測定されます。したがって、例えば、100タイムステップごとに熱力学出力を印刷している場合、これら2つのキーワードは過去100ステップに対する時間とタイムステップの速度を継続的に出力します。tpcpuキーワードはタイムステップサイズの変更（例：[fix dt/reset]()コマンドの使用による）を追跡しません。

cpuremainキーワードは、現在の実行における残りのCPU時間を、これまでの経過時間に基づいて推定します。この推定が良いものとなるのは、残りの実行でのCPU時間/タイムステップがこれまでのタイムステップと似ている場合です。最初のタイムステップでは、推定に使用できる履歴がないため、値は0.0になります。minimizeコマンドによって実行された最適化実行では、この推定はmaxiterパラメータに基づいており、最適化が最大許容回数の反復で進行することを前提としています。

partキーワードは、マルチレプリカやマルチパーティションシミュレーションで役立ち、この出力とファイルがどのパーティションに対応しているかを示します。また、このキーワードは[variable]()内で使用して、パーティション固有の出力用のファイル名に追加するためにも利用できます。マルチパーティションモードでの実行の詳細については、マニュアルの第7節を参照してください。

fmaxおよびfnormキーワードは、エネルギー最適化の進行状況を監視するために役立ちます。fmaxキーワードは、システム内の任意の原子に対する任意の次元での最大力、またはシステム全体の力ベクトルの無限ノルムを計算します。fnormキーワードは、力ベクトルの2ノルム、または長さを計算します。

cella、cellb、cellc、cellalpha、cellbeta、cellgammaキーワードは、結晶の周期的単位セルを定義する通常の結晶学的量に対応します。これらの量をLIGGGHTS(R)-PUBLICの内部セル次元であるlx、ly、lz、yz、xz、xyを用いて定義した三斜晶周期セルに関する幾何学的説明については、ドキュメントのこのセクションを参照してください。

c_ID、c_ID[I]、c_ID[I][J]キーワードは、計算によって得られたグローバル値を出力するために使用されます。computeのドキュメントページで説明されているように、[compute]()はグローバル値、原子ごとの値、またはローカル値を計算できます。このコマンドで参照できるのはグローバル値のみですが、原子ごとの計算値は[variable]()内で参照でき、thermo_style customでその変数を参照できます。

キーワード内のIDは、入力スクリプト内で他の場所で定義されたcomputeの実際のIDに置き換える必要があります。詳細については、computeコマンドを参照してください。computeがグローバルなスカラー、ベクトル、または配列を計算する場合、キーワードに0、1、または2つの括弧がついた形式で、計算からスカラー値を参照します。

いくつかのcomputeは温度のような「集中的な」グローバル量を計算しますが、他のcomputeは運動エネルギーのような「拡張的な」グローバル量を計算し、これらは計算グループ内のすべての原子に対して合計されます。集中的な量は、thermo_style customで正規化されることなく直接出力されます。拡張的な量は、出力時にシミュレーション全体の原子数（計算グループ内の原子数ではなく）で正規化されることがありますが、これは使用されている[thermo_modify norm]()オプションに依存します。

f_ID、f_ID[I]、f_ID[I][J]キーワードは、fixによって計算されたグローバル値を出力するために使用されます。[fix]()のドキュメントページで説明されているように、fixはグローバル値、原子ごとの値、またはローカル値を計算できます。このコマンドで参照できるのはグローバル値のみですが、原子ごとのfix値は[variable]()内で参照でき、thermo_style customでその変数を参照できます。

キーワード内のIDは、入力スクリプト内で他の場所で定義されたfixの実際のIDに置き換える必要があります。詳細については、fixコマンドを参照してください。fixがグローバルなスカラー、ベクトル、または配列を計算する場合、キーワードに0、1、または2つの括弧がついた形式で、fixからスカラー値を参照します。

いくつかのfixは、タイムステップのサイズのような「集中的な」グローバル量を計算し、他のfixはエネルギーのような「拡張的な」グローバル量を計算して、これらはfixグループ内のすべての原子に対して合計されます。集中的な量は、thermo_style customで正規化されることなくそのまま出力されます。拡張的な量は、出力時にシミュレーション全体の原子数（fixグループ内の原子数ではなく）で正規化されることがあり、これは使用されている[thermo_modify norm]()オプションに依存します。

v_nameキーワードは、変数の現在の値を出力するために使用されます。キーワード内の名前は、入力スクリプト内で他の場所で定義された変数名に置き換える必要があります。参照できるのは、equalスタイルの変数のみです。詳細については、[variable]()コマンドを参照してください。equalスタイルの変数は、原子ごとのプロパティや熱力学的キーワードを参照したり、評価時に他のcompute、fix、または変数を呼び出すことができるため、これは非常に一般的な熱力学的出力を作成する手段となります。

なお、equalスタイルの変数は「集中的な」グローバル量と見なされるため、thermo_style customで正規化されることなくそのまま出力されます。もしそうでない場合は、変数式内でnatomsで割ることを含めることができます。

## 制限事項
このコマンドは、シミュレーションボックスがread_data、read_restart、またはcreate_boxコマンドによって定義された後に実行する必要があります。

## 関連コマンド
[thermo](), [thermo_modify](), [fix_modify](),

## デフォルト
```
thermo_style one
```
