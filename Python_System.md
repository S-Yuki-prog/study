---

## 💻 開発・学習活動 (Development & Learning)

### University Programming Exercises
大学の講義（プログラミング演習）で作成したプログラムや、アルゴリズムの学習内容となっています。

**使用したプログラミング言語** :　Python<br>

情報工学基礎ユニットという実験科目で作成したプログラムになります。

---

## 単純パーセプトロンの作成と学習プログラム
>活性化関数（ステップ関数の作成）…計算された合計値が一定の閾値（0）を超えたかどうかを判定

      def _activate_step(y):
          if y >= 0:
              return 1
          else:
              return 0
              
>重み付き和の計算 …関数において、各入力信号に重みを掛け合わせ、最後にバイアスを加算している。

      def _two_input_weight(x1, x2, w1, w2, b):
          return x1 * w1 + x2 * w2 + b
---
>単純パーセプトロン関数

      def two_input_perceptron(x1, x2, w1, w2, b):
          _y = _two_input_weight(x1, x2, w1, w2, b)
          y = _activate_step(_y)
          return y

単純パーセプトロンの作成に当たって活性化関数と重み付き和の計算を含める必要がある。

---
