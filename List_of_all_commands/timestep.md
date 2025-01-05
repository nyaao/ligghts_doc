# timestep command

## 構文
```
timestep dt
```
dt = タイムステップサイズ (time units)

## 例
```
timestep 2.0
timestep 0.003
```

## 説明
分子動力学シミュレーションの後続のタイムステップサイズを設定します。時間単位については、unitsコマンドで説明があります。タイムステップのデフォルト値は、シミュレーションの単位の選択によっても異なります。デフォルト値は以下を参照してください。

runスタイルがrespaの場合、dtは外部ループ（最も大きい）タイムステップのタイムステップサイズです。

## 制限事項
none

## 関連コマンド
[fix dt/reset](), [run](), [run_style]() respa, [units]()

## デフォルト
timestep = 0.005 tau for units = lj  
timestep = 1.0 fmsec for units = real  
timestep = 0.001 psec for units = metal  
timestep = 1.0e-8 sec (10 nsec) for units = si or cgs  
