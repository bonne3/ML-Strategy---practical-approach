Coursera DL "Structuring Machine Learning Projects"

１．教師あり学習でシステムを動かす場合の手順

①Training Setで精度が高い（＝Training Setがコスト関数に良くFitする）、すなわち一定水準、例えば人間レベルの正確度を確保しているかチェック
→　上手にいかなかった場合はBigger Neural Network、Better Optimization Algorithms like Adamを試す

②Dev Setで精度が高い（＝Dev Setがコスト関数に良くFitするかチェック）
→　上手にいかなかった場合はRegularization、Bigger Training Setを試す

③Test Setで精度が高い（＝Test Setがコスト関数に良くFitする）
→　上手にいかなかった場合は、Dev SetにOver fitしているのでBigger Dev Setを準備する

④実世界で上手に動くかチェック
→　上手にいかなかったら、１）Dev Setを変える（Dev SetのDistributionがTest Setと異なるから）、２）Cost Functionを変える（コスト関数が実際に測りたいものをキャッチできていない）

【ポイント】Ianに伝えるべきなのは、Training/Dev/Testセットのどこで精度が低かったのか、ということ。

２．結果の評価軸の設定方法

①評価基準は一つにする（single number evaluation metrics)
たとえば、PrecisionとRecallを比べてもきりがない（※）ので、この2つを統合した評価軸を使う。
ここでいうと、F1（PrecisionとRecallのHarmonic Mean）。

※　Precision：猫の写真判定をさせるケースでいうと、猫の写真とAIが判定したもののうち、実際に猫である確率（要は猫と言って、あたった確率）。
※　Recall：猫の写真判定をさせるケースでいうと、本当の猫の写真のうち、猫と判定できた確率（要は全部の猫のうち、あてられた確率）

→Classifierを10個試したときに、どれを使い続けるのがいいのかは、F1を使って判定する。これにより効率の良いアルゴリズムチェックで後続作業をすることが可能。

→ここでいうF1が、Single Number Evaluation Metric

②評価基準が一つに絞れない場合
評価基準（メトリクス）が複数nある場合。
そのうちの1つをOptimizing、N-1はSatisficingとする

例：AccuracyをOptimizing、Running TimeをSatisficing
ダメな例： Cost = Accuracy - 0.5 *Running Time
Goodな例： Maximize accuracy subject to running time ≦ 100ms。Accuracy RateがOptimizaing Metricで、Running TimeがSatisficing Metric。

その他に、Wakewords（OK Google）の場合は以下の通り。
AccuracyがOptimizing Metric、
A number of false positives（本当はOK Googleと言っていないのに、AIが勘違いした例） per dayがSatisficing Metric。
