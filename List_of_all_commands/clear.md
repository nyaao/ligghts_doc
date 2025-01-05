# clearcommand

## 構文
```
clear
```
## 例
```
(commands for 1st simulation)
clear
(commands for 2nd simulation)
```

## 説明
このコマンドは、すべての原子を削除し、すべての設定をデフォルト値にリセットし、LIGGGHTS(R)-PUBLIC によって割り当てられたすべてのメモリを解放します。一度 clear コマンドが実行されると、以下に記載されている例外を除いて、LIGGGHTS(R)-PUBLIC が初期状態に戻るのと同じ状態になります。このコマンドにより、1つの入力スクリプトから複数のジョブを連続して実行することが可能になります。

clear コマンドでは影響を受けない設定は以下の通りです：作業ディレクトリ（[shell]() コマンド）、ログファイルの状態（[log]() コマンド）、エコーの状態（[echo]() コマンド）、および入力スクリプトの変数（[variable]() コマンド）。

## 制限事項
none

## 関連コマンド
none

## デフォルト
none
