# compute nparticles/tracer/region command

## 構文
```
compute ID group-ID nparticles/tracer/region general_keyword general_values
```
- ID、group-IDはcomputeコマンドに記載されています。
- nparticles/tracer/region = このcomputeコマンドのスタイル名
- 一般的なキーワードと値はcomputeに記載されています。
- region_count = 必須キーワード
- region-ID = 粒子がカウントされるために存在する必要がある領域のID
- tracer = 必須キーワード
- tracer-ID = fix property/atom/tracerタイプのfixのID
- argsにはゼロ個以上のキーワード/値ペアを追加できます。
- キーワード = periodicまたはreset_marker

periodic値 = dim image  
dim = x、y、またはz  
image = 粒子がカウントされるために存在する必要がある画像（任意の整数またはall）  
reset_marker値 = yesまたはno  
yes = 粒子をカウント後にマークを解除  
no = 粒子をカウント後にマークを解除しない  

## 例
```
compute nparticles all nparticles/tracer/region region_count count tracer tr periodic z -1
```

## 説明
指定されたregion_countキーワードで指定された領域にいる、マークされた粒子とマークされていない粒子の数と質量を計算する計算を定義します。粒子はカウントされるために「group-ID」グループに含まれている必要があります。

注意すべきは、[fix property/atom/tracer]()または[fix property/atom/tracer/stream]()コマンドによってマークされた粒子のみがカウントされるという点です。そのため、このようなfixの有効なIDをtracerキーワードを通じて提供する必要があります。

reset_markerキーワードは、粒子が一度カウントされた後にマークを解除するかどうか（デフォルトでは解除）を制御します。

[!WARNING]複数のcompute nparticles/tracer/regionコマンドが同じ[fix property/atom/tracer]()コマンドで動作している場合、最初の計算がマーカー値をリセットすると、2番目の計算ではそれらの粒子はカウントされません。

periodicキーワードを使用すると、周期的なシミュレーションで特定の画像にいる粒子に対してカウントやマーク解除を制限できます。例えば、次のように使用します：

```
periodic z +2
```

これは、粒子がz-画像#2にいる場合のみカウントされることを意味します。デフォルトでは、すべての粒子が周期的な画像に関係なくカウントされ、マーク解除されます。

[!WARNING]現在、このコマンドはperiodicキーワードを使用した1つの周期境界制限のみをサポートしています。periodicキーワードが複数回使用された場合、最後の設定が適用されます。

## 出力情報
この計算は、以下の情報を含むグローバルベクトルを計算します（括弧内の番号はベクトルIDを示します）：

1. 領域内の（マークされた + マークされていない）粒子の総数
2. 領域内のマークされた粒子の数
3. 領域内の（マークされた + マークされていない）粒子の総質量
4. 領域内のマークされた粒子の質量

LIGGGHTS(R)-PUBLICの出力オプションの概要については、このセクションを参照してください。

## 制限事項
現在、periodic キーワードを使って設定できる周期的制限は1つのみです。

## 関連コマンド
[fix property/atom/tracer]()

## デフォルト
reset_marker = yes、periodic はデフォルトでオフです。