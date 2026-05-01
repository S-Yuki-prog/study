  ---

## 💻 開発・学習活動 (Development & Learning)

### University Programming Exercises
大学の講義（プログラミング演習）で作成したプログラムや、アルゴリズムの学習内容となっています。

**使用したプログラミング言語** :　C言語<br>

他にも様々なプログラムを作成しましたが、代表的なものとして以下のプログラムを提示します。

---
## TCP / IPによるネットワークコミュニケーション
TCP / IPを利用したネットワークプログラミングを行うために、ソケットの基礎を学びクライアント・サーバ型のネットワークコミュニケーションプログラムを作成する。<br>
コマンドプロンプト( x85 Native Tools Command Prompt for VS 2022 )を2画面開き、クライアント、サーバの動き方を学んだ。<br>

### 実行動画（GIF）
↓上のコマンドプロンプトがサーバの動作を行う。
<img width="957" height="1270" alt="Jkcg6ABeiBsCFpDgEHbo1777613313-1777613409" src="https://github.com/user-attachments/assets/1a9ea089-84d4-417f-a682-5671829a533e" />
↑下のコマンドプロンプトがクライアントの動作を行う。

---
server5.c

      #include <stdio.h>
      #include <stdlib.h>
      #include <string.h>
      #include <winsock2.h>
      #include <ws2tcpip.h>

      #pragma comment(lib, "ws2_32.lib")

      // ユーザ認証を行う関数
      int authenticate(int connfd) {
          char user[1024], pass[1024];
    
          // ユーザ名とパスワードを受信
          recv(connfd, user, sizeof(user), 0);
          recv(connfd, pass, sizeof(pass), 0);

          // ユーザ名"jikken", パスワード"tcpip"
          if (strcmp(user, "jikken") == 0 && strcmp(pass, "tcpip") == 0) {
              send(connfd, "SUCCEED", 8, 0); // 成功を送信
              return 1;
          } else {
              send(connfd, "FAILURE", 8, 0); // 失敗を送信
              return 0;
          }
      }

      int main() {
          WSADATA wsaData;
          int sockfd, connfd;
          struct sockaddr_in servaddr, cliaddr;
          int len;
          char buf[1024];

          WSAStartup(MAKEWORD(2, 0), &wsaData);
          sockfd = socket(AF_INET, SOCK_STREAM, 0);

          servaddr.sin_family = AF_INET;
          servaddr.sin_port = htons(10000);
          servaddr.sin_addr.s_addr = INADDR_ANY;
          int opt = 1;
          setsockopt(sockfd, SOL_SOCKET, SO_REUSEADDR, (const char *)&opt, sizeof(opt));

          bind(sockfd, (struct sockaddr *)&servaddr, sizeof(servaddr));
          listen(sockfd, 5);

          len = sizeof(cliaddr);
          connfd = accept(sockfd, (struct sockaddr *)&cliaddr, (socklen_t *)&len);

          // --- ステップA: 認証処理 ---
          if (authenticate(connfd)) {
              printf("認証成功: サービスを開始します\n");
              // --- ステップB: 実験4の動作 (メッセージ受信ループ) ---
              while (1) {
                  int n = recv(connfd, buf, sizeof(buf) - 1, 0);
                  if (n <= 0) break;
                  buf[n] = '\0';
                  printf("%s\n", buf);
                  if (strcmp(buf, "BYE!") == 0) break;
              }
          } else {
              printf("認証失敗: 接続を終了します\n");
          }

          closesocket(connfd);
          closesocket(sockfd);
          WSACleanup();
          return 0;
      }


---
client5.c

      #include <stdio.h>
      #include <stdlib.h>
      #include <string.h>
      #include <winsock2.h>

      #pragma comment(lib, "ws2_32.lib")

      // ログイン入力を担当する関数
      int login(int sockfd) {
          char user[1024], pass[1024], res[10];

          printf("Login: ");
          fgets(user, sizeof(user), stdin);
          user[strcspn(user, "\r\n")] = 0;

          printf("Password: ");
          fgets(pass, sizeof(pass), stdin);
          pass[strcspn(pass, "\r\n")] = 0;

          // サーバへ送信
          send(sockfd, user, (int)strlen(user) + 1, 0);
          send(sockfd, pass, (int)strlen(pass) + 1, 0);

          // 結果を受信
          recv(sockfd, res, sizeof(res), 0);

          if (strcmp(res, "SUCCEED") == 0) {
              printf("認証成功！\n");
              return 1;
          } else {
              printf("認証失敗\n");
              return 0;
          }
      }

      int main() {
          WSADATA wsaData;
          int sockfd;
          struct sockaddr_in servaddr;
          char input[1024];

          WSAStartup(MAKEWORD(2, 0), &wsaData);
          sockfd = socket(AF_INET, SOCK_STREAM, 0);

          servaddr.sin_family = AF_INET;
          servaddr.sin_port = htons(10000);
          servaddr.sin_addr.s_addr = inet_addr("127.0.0.1");

          connect(sockfd, (struct sockaddr *)&servaddr, sizeof(servaddr));

          if (login(sockfd)) {
              // --- ステップB: 実験4の動作 (メッセージ送信ループ) ---
              while (1) {
                  printf("入力 : ");
                  fgets(input, sizeof(input), stdin);
                  input[strcspn(input, "\r\n")] = 0;
                  send(sockfd, input, (int)strlen(input) + 1, 0);
                  if (strcmp(input, "BYE!") == 0) break;
              }
          }

          closesocket(sockfd);
          WSACleanup();
          return 0;
      }

---

このプログラムで使用した関数一覧

| 関数名 | ヘッダファイル | 概要 |
| :--- | :--- | :--- |
| **socket** | winsock2.h | 通信のためのソケットを生成する |
| **bind** | winsock2.h | ソケットにローカルのアドレス（IPとポート番号）を割り当てる |
| **listen** | winsock2.h | ソケットを接続待ち状態（サーバー側）にする |
| **accept** | winsock2.h | クライアントからの接続要求を承認し、通信用ソケットを新しく作る |
| **connect** | winsock2.h | サーバーに対して接続を要求する（クライアント側） |
| **recv** | winsock2.h | 接続されたソケットからデータを受信する |
| **send** | winsock2.h | 接続されたソケットへデータを送信する |
| **close** | winsock2.h | ソケットをクローズし、リソースを解放する |
| **inet_addr** | winsock2.h | IPv4ドット数記法をネットワークバイトオーダの数値に変換する |
| **perror** | stdio.h | 直前に発生したシステムエラーの内容を標準エラー出力に表示する |
| **bzero** | string.h | 指定されたメモリ領域を0で埋める（Windows環境ではmemsetが一般的） |
| **strcpy** | string.h | 文字列を別のメモリ領域にコピーする |
| **strcmp** | string.h | 2つの文字列を比較する |
| **strlen** | string.h | 文字列の長さ（バイト数）を算出する |


