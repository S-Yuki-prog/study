---

## 💻 開発・学習活動 (Development & Learning)

### University Programming Exercises
大学の講義（プログラミング演習）で作成したプログラムや、アルゴリズムの学習内容となっています。

**使用したプログラミング言語** :　Python<br>

情報工学基礎ユニットという実験科目で作成したプログラムになります。

---

# 単純パーセプトロンの作成と学習プログラム
## 単純パーセプトロンの作成
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
## 学習プログラム


>パラメータの差分を計算する関数

      def get_weight_deltas(x1, x2, w1, w2, b, t):
          # ANDゲートの推論（学習時は現在の重みを使用）
          y = two_input_perceptron(x1, x2, w1, w2, b)
          db = t - y
          dw1 = db * x1
          dw2 = db * x2
          return dw1, dw2, db


>差分を更新する関数

      def check_loss(dw1, dw2, db):
          return (dw1 == 0) and (dw2 == 0) and (db == 0)

      def update_weight(x1, x2, w1, w2, b, t):
          dw1, dw2, db = get_weight_deltas(x1, x2, w1, w2, b, t)
          loss = check_loss(dw1, dw2, db)
          # 重みの更新
          w1 += dw1
          w2 += dw2
          b += db
          return w1, w2, b, loss


>学習の実行（ANDゲートの例）

      def train_perceptron():
          x_in = [[0, 0], [0, 1], [1, 0], [1, 1]]
          t_in = [0, 0, 0, 1]  # ANDゲートの正解ラベル
    
          w1, w2, b = 0, 0, 0  # 初期値
          epoch = 30
    
          for j in range(epoch):
              classified = True
              for i in range(len(x_in)):
                  x1, x2 = x_in[i]
                  t = t_in[i]
                  w1, w2, b, loss = update_weight(x1, x2, w1, w2, b, t)
                  classified *= loss
        
              if classified:
                  print(f"学習完了（{j+1}エポック目）")
                  print(f"結果: w1={w1}, w2={w2}, b={b}")
                  break


---
>実行

      train_perceptron()

>実行結果

      学習完了（6エポック目）
      結果: w1=2, w2=1, b=-3

ANDゲートを成立させるためのもの。<br>
この出力された結果（結果: w1=2, w2=1, b=-3）は下記のように示すことができる。<br>
$$y = 2x_1 + 1x_2 - 3$$ <br>
この式が0になるものを考える。<br>
<br>
この $x_1, x_2$ は入力で、それぞれ0、もしくは1が入力されるものとする。<br>
ANDは入力 $x_1, x_2$ が(1,1)の場合、出力は1、それ以外の(0,0)、(0,1)、(1,0)の出力は全て0となる。<br>
<br>
実際に $x_1, x_2$ の入力が(1,1)の場合を計算する。<br>
$$y = 2x_1 + 1x_2 - 3$$<br>
2(1)+1(1)+(-3)=0<br>
<br>
w1、w2が「重み」、bが「バイアス」といった構造に仕上がり、<br>
単純パーセプトロンの作成と学習プログラムが実現できた。<br>


---



