Coursera DL "Structuring Machine Learning Projects"

１．教師あり学習でシステムを動かす場合の手順

　①　Training Setで精度が高い（＝Training Setがコスト関数に良くFitする）か、具体的には例えば人間レベルの精度を確保しているかチェック

　→　上手にいかなかった場合はBigger Neural Network、Better Optimization Algorithms like Adamを試す

　②　Dev Setで精度が高い（＝Dev Setがコスト関数に良くFitする）かチェック

　→　上手にいかなかった場合はRegularization、Bigger Training Setを試す

　③　Test Setで精度が高い（＝Test Setがコスト関数に良くFitする）かチェック

　→　上手にいかなかった場合は、Dev SetにOver fitしているのでBigger Dev Setを準備する

　④　実世界で上手に動くかチェック

　→　上手にいかなかったら、１）Dev Setを変える（Dev SetのDistributionがTest Setと異なるから）、２）Cost Functionを変える（コスト関数が実際に測りたいものをキャッチできていない）

【ポイント】Ianに伝えるべきなのは、Training/Dev/Testセットのどこで精度が低かったのか、ということ。


２．結果の評価軸の設定方法

　①　評価基準は一つにする（single number evaluation metrics)

　たとえば、アルゴリズムを複数用意したとして、各アルゴリズムのPrecisionとRecallを比べてもきりがない（※）ので、この2つを統合した評価軸を使う。
　ここでいうと、F1（PrecisionとRecallのHarmonic Mean）。

　※　Precision：猫の写真判定をさせるケースでいうと、猫の写真とAIが判定したもののうち、実際に猫である確率（要は猫と言って、当たった確率）。

　※　Recall：猫の写真判定をさせるケースでいうと、本当の猫の写真のうち、猫と判定できた確率（要は全部の猫の写真のうち、AIが当てられた確率）

　→　Classifier（要はアルゴリズム）を10個試したときに、どれを使い続けるのがいいのかは、F1を使って判定する。これにより効率の良いアルゴリズムを選んで、後続作業をすることが可能。

　→　ここでいうF1が、Single Number Evaluation Metric（RecallとPrecisionという二つのMetricを、一つにまとめたということ）

　②　評価基準が一つに絞れない場合

　評価基準（メトリクス）が複数Nある場合。
　そのうちの1つをOptimizing基準、N-1はSatisficing基準とする

　例：基準がモデルを回したときの正確性（Accuracy）と所要時間（Runnin Time）の2つある場合。

　ダメな例： Cost = Accuracy - 0.5 * Running Timeという形で、式にしてしまうこと

　Goodな例： Maximize accuracy subject to running time ≦ 100ms。すなわち、Accuracy RateがOptimizaing Metricで、Running TimeがSatisficing Metric。Running Timeは100ms以下であればOKということ（60msでも80msでも、100msであればOK。60msのAccuracyが80％で、80msのAccuracyが70％なら、Accuracyが高い60ms＋80％を選ぶ、ということ）

　その他の例：Wakewords（OK Google）の場合は以下の通り。AccuracyがOptimizing Metric、A number of false positives（本当はOK Googleと言っていないのに、AIが勘違いした例） per dayがSatisficing Metric。


３．Training/Dev/Testセット

　お客様からお預かりしたデータは、教師データの作成（Training）、データの1次チェック（Dev）、データの最終チェック（Test）用に3つにわける。
　500時間以上のデータがあれば、その割合は９０％：５％：５％。データが500時間未満ならば６０％：２０％：２０％。

　Dev/Test Setsは同じData Distributionにする必要がある。

　例：Dev Data setをUS,UK,Other Europe, South America

　Test Data setをIndia, China, Other Asia, Australia

　の各々からデータセットを作るのはダメ。Dev/Test何れも、すべての国のデータをシャッフルして作成する。

　→ Choose a dev set and test set to reflect data you expect to get in the future and consider important to do well on


４．結果の評価の仕方

　まず、統計学でランダムなテストを行った時の最小エラー確率のことをBayes Optimal Errorという。
　この定義上、Bayes Optimal Errorはbest possible error, no one can surpassである。

　Human levelはBayes Error以下の水準となる。

　機械学習の精度が、Human Levelより低い場合は改善の余地がある。例えば、より多くの教師データを集める、人間の精度が高い理由を定性的に分析する、MLにBias/Varianceがないか確認するなど。

　（具体例）

　人間のError、Training Error、Dev Errorが
　（１）各1％、8％、10％の場合

　→Trainingを改善する必要あり。

　よって、Biasの改善の問題（＝Bigger NN、Better Optimization Algorithms, Training Setの訓練を長時間する、ハイパーパラメーターの調整）

　ここで、Human（≒Bayes Error）とTraining Errorの差をAvoidable Biasと呼ぶ。

　（２）各7.5％、8％、10％の場合

　→　Devを改善する必要あり。Varianceの問題（＝Regularization（L2、Dropout、Data Augmentation）、NN Architectureの変更）

　Training ErrorとDev Errorの差は、Variance（不変）
