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


３．Training/Dev/Testセット

お客様からお預かりしたデータは、教師データの作成（Training）、データの1次チェック（Dev）、データの最終チェック（Test）用に3つにわける。
500時間以上のデータがあれば、その割合は９０％：５％：５％。データが500時間未満ならば６０％：２０％：２０％。

Dev/Test Setsは同じData Distributionにする必要がある。

例：DevをUS,UK,Other Europe, South America

TestをIndia, China, Other Asia, Australia

の各々からデータセットを作るのはダメ。Dev/Test何れも、すべての国のデータをシャッフルして作成する。

→ Choose a dev set and test set to reflect data you expect to get in the future and consider important to do well on


４．結果の評価の仕方

まず、統計学でランダムなテストを行った時の最小エラー確率のことをBayes Optimal Errorという。
この定義上、Bayes Optimal Errorはbest possible error, no one can surpassである。

Human levelはBayes Error以下の水準となる。

機械学習の精度は、Human Levelより低ければ、処置のしようがある。

例：教師データを集める、人間の精度が高い理由を分析する、MLにBias/Varianceがないか確認するなど

具体例

人間のError、Training Error、Dev Errorが
（１）各1％、8％、10％の場合

→Trainingを改善する必要あり。

よって、Biasの改善の問題（＝Bigger NN、Better Optimization Algorithms, Training Setの訓練を長時間する、ハイパーパラメーターの調整）

ここで、Human（≒Bayes Error）とTraining Errorの差をAvoidable Biasと呼ぶ。

（２）各7.5％、8％、10％の場合

→　Devを改善する必要あり。Varianceの問題（＝Regularization（L2、Dropout、Data Augmentation）、NN Architectureの変更）

Training ErrorとDev Errorの差は、Variance（不変）
