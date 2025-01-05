# compute cna/atom command

## 構文
```
compute ID group-ID cna/atom general_keyword general_values cutoff
```

- ID、group-ID は compute コマンドで文書化されています。
- cna/atom = この compute コマンドのスタイル名
- general_keywords general_values は compute コマンドで文書化されています。
- cutoff = 最近接原子のカットオフ距離（距離の単位）

## 例
```
compute 1 all cna/atom 3.08
```

## 説明
この計算は、グループ内の各原子に対して CNA（共通隣接原子解析）パターンを計算します。固体システムにおいて、CNAパターンは原子の周りの局所的な結晶構造を測定するのに有用な手法です。CNAの方法論は（Faken, Jonsson, Comput Mater Sci, 2, 279 (1994).）および（Tsuzuki, Branicio, Rino, Comput Phys Comm, 177, 518 (2007).）で説明されています。

現在、LIGGGHTS(R)-PUBLIC が認識するCNAパターンは5種類です。

- fcc = 1
- hcp = 2
- bcc = 3
- icosohedral = 4
- unknown = 5

CNAパターンの値は、指定された計算グループに含まれない原子には0となります。通常、CNA計算は単一成分システムに対してのみ実行するべきであることに注意してください。

CNA計算は、指定されたカットオフ値に敏感です。原子の適切な最近接原子が、想定される結晶構造に基づいてカットオフ距離内で見つかることを確認する必要があります。例えば、完全なFCCおよびHCP結晶の場合は12個の最近接原子、完全なBCC結晶の場合は14個の最近接原子です。これらの式を使用して、適切なカットオフ距離を得ることができます。

$$
r_c^{fcc}=\frac{1}{2}\left( \frac{\sqrt2}{2}+1\right)a\simeq0.8536a
$$

$$
r_c^{bcc}=\frac{1}{2}\left( \sqrt2+1\right)a\simeq1.207a
$$

$$
r_c^{hcp}=\frac{1}{2}\left(1+  \sqrt{\frac{4+2x^2}{3}}\right)a
$$

ここで、a は対象の結晶構造の格子定数であり、HCPの場合、x = (c/a) / 1.633 で、1.633 はHCP結晶の理想的な c/a 比です。

また、LIGGGHTS(R)-PUBLIC のCNA計算では、所有された原子の隣接原子を使用してゴースト原子の最近接原子を見つけるため、次の関係も満たす必要があることに注意してください。

$$
Rc + Rs \geqq 2*cutoff
$$

ここで、Rc はポテンシャルのカットオフ距離、Rs は neighbor コマンドで指定されたスキン距離、cutoff は compute cna/atom コマンドで使用される引数です。もしこれが満たされない場合、LIGGGHTS(R)-PUBLIC は警告を発行します。

この量を計算するために必要な近隣リストは、計算が実行されるたびに構築されます（例えば、原子のスナップショットがダンプされるたびに）。そのため、この量をあまり頻繁に計算・ダンプすることは非効率的であり、複数の compute / dump コマンドを使用して、各コマンドが cna/atom スタイルである場合も非効率的です。

## 出力情報
この計算は、各原子に対してベクトルを計算します。このベクトルは、compute からの各原子の値を入力として使用する任意のコマンドでアクセスできます。LIGGGHTS(R)-PUBLIC の出力オプションの概要については、セクション 15 を参照してください。

各原子のベクトル値は、上記の通り、0 から 5 の間の数値になります。

## 制限事項
none

## 関連コマンド
[compute centro/atom]()

## デフォルト
none