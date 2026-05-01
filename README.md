---

## 💻 開発・学習活動 (Development & Learning)

### University Programming Exercises
大学の講義（プログラミング演習）で作成したプログラムや、アルゴリズムの学習内容となっています。

**学んだプログラミング言語** :　Java の例<br>

他にも様々なプログラムを作成しましたが、代表的なものとして以下のプログラムを提示します。

---

## 購入によるポイントの付与

会員関係のクラスを、抽象クラスを利用するシステムで、 <br>
・Memberを抽象クラスとして作成します。このクラスのメソッドbuyは抽象メソッドに<br>
・Memberの派生クラスとして、NormalMemberクラスとPremiumMemberクラスを作成し、buyメソッドをオーバーライド。 <br>

NormalMember、PremiumMemberの会員の情報を二つに分け、ポイントの付与情報も二つに分けるシステム。<br>
入力：商品名を入力し、購入金額を入力する<br>
出力：今回の獲得ポイントを表示し、合計で何ポイントになるかを表示。最後に二人の追加された後の合計ポイントを表示<br>

### 実行動画（GIF）
<img width="960" height="610" alt="jWV5WTHpbRuGtMKT49cc1777597965-1777598549" src="https://github.com/user-attachments/assets/54734ef7-a7bc-40c6-86bd-100e94139c9e" />



        package java1_7;

        import java.util.Scanner;

        public class Kadai7_3 {
          public static void main(String[] args) {
          Scanner scanner = new Scanner(System.in);

          // 各メンバーのインスタンスを初期化
          NormalMember nm = new NormalMember("N123", "Amuro", 400);
          PremiumMember pm = new PremiumMember("P555", "Akai", 800);

          // --- NormalMemberの購入処理 ---
          System.out.println(nm.getCustomerName() + "さん");
          System.out.print("商品名？ >");
          String nmProductName = scanner.nextLine(); // 商品名の入力
          System.out.print("購入金額？ >");
          int nmPrice = scanner.nextInt(); // 購入金額の入力
          scanner.nextLine(); // nextInt()で読み残した改行の処理

          nm.buy(nmProductName, nmPrice); // 購入メソッド呼び出し
          System.out.println(); // 空行

          // --- PremiumMemberの購入処理 ---
          System.out.println(pm.getCustomerName() + "さん");
          System.out.print("商品名？ >");
          String pmProductName = scanner.nextLine(); // 商品名の入力
          System.out.print("購入金額？ >");
          int pmPrice = scanner.nextInt(); // 購入金額の入力
          scanner.nextLine(); // nextInt()で読み残した改行の処理

          pm.buy(pmProductName, pmPrice); // 購入メソッド呼び出し
          System.out.println(); // 空行

          // --- 最終的なメンバー情報の表示 ---
          System.out.println(nm.toString()); // toStringメソッドを呼び出し
          System.out.println(pm.toString()); // toStringメソッドを呼び出し

          scanner.close(); // Scannerを閉じる
        }
      }
---
### Member.java

      package java1_7;

      public abstract class Member {
        private String customerID;   // 顧客ID
        private String customerName; // 顧客氏名
        private int point;           // ポイント
        public final int rate = 1000; // 1ポイントを獲得する金額（変更不可）

        public Member(String customerID, String customerName, int point) {
          this.customerID = customerID;
          this.customerName = customerName;
          this.point = point;
        }

        // 顧客氏名のgetter
        public String getCustomerName() {
          return customerName;
        }

        // ポイントのgetter
        public int getPoint() {
            return point;
        }

        // ポイントのsetter（ポイントを加算するために使用）
        public void addPoint(int pointsToAdd) {
            this.point += pointsToAdd;
        }

        // 抽象メソッド
        public abstract void buy(String productName, int price);
      }

---
### NormalMember.java

      package java1_7;

      public class NormalMember extends Member {

          public NormalMember(String customerID, String customerName, int point) {
              super(customerID, customerName, point); // 基底クラスのコンストラクターを呼び出す
          }

          @Override
          public String toString() {
              return "NormalMember " + getCustomerName() + "さんは、現在" + getPoint() + "ポイント";
          }

          @Override
          public void buy(String productName, int price) {
              // 通常会員のポイント計算
              int earnedPoint = price / rate; // Memberクラスのrateを使用
              addPoint(earnedPoint); // 合計ポイントに加算

              System.out.println(getCustomerName() + " が " + productName + "を" + price + "円で買います。");
              System.out.println("今回獲得ポイントは " + earnedPoint + "ポイントで、合計" + getPoint() + "ポイントになりました。");
          }
      }

---
### PremiumMember.java

      package java1_7;

      public class PremiumMember extends Member {
          public final int premiumRate = 200; // PremiumMemberが1ポイントを獲得する金額

          public PremiumMember(String customerID, String customerName, int point) {
              super(customerID, customerName, point); // 基底クラスのコンストラクターを呼び出す
          }

          @Override
          public String toString() {
              return "PremiumMember " + getCustomerName() + "さんは、現在" + getPoint() + "ポイント";
          }

          @Override
          public void buy(String productName, int price) {
              // PremiumMemberのポイント計算
              // MemberのrateによるポイントとpremiumRateによるポイントの両方を加算
              int earnedPointByRate = price / rate; // Memberクラスのrateを使用
              int earnedPointByPremiumRate = price / premiumRate;
              int totalEarnedPoint = earnedPointByRate + earnedPointByPremiumRate;

              addPoint(totalEarnedPoint); // 合計ポイントに加算

              System.out.println(getCustomerName() + " が " + productName + "を" + price + "円で買います。");
              System.out.println("今回獲得ポイントは " + totalEarnedPoint + "ポイントで、合計" + getPoint() + "ポイントになりました。");
            }
    }


---
© 2026 Yuki Seki
