Coursera DL "Structuring Machine Learning Projects"

教師あり学習でシステムを動かす場合の手順

①Training Setで精度が高い（＝Training Setがコスト関数に良くFitする。）すなわち、一定水準、例えば人間レベルの正確度を確保しているかチェック
→　上手にいかなかった場合はBigger Network、Better Optimization Algorithms like Adamを試す

②Dev Setで精度が高い（＝Dev Setがコスト関数に良くFitするかチェック）
→　上手にいかなかった場合はRegularization、Bigger Training Setを試す

③Test Setで精度が高い（＝Test Setがコスト関数に良くFitする）
→　上手にいかなかった場合は、Dev SetにOver fitしているのでBigger Dev Setを準備する

④実世界で上手に動くかチェック
→　上手にいかなかったら、１）Dev Setを変える（Dev SetのDistributionがTest Setと異なるから）、２）Cost Functionを変える（コスト関数が実際に測りたいものをキャッチできていない）
