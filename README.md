# ParallelFor

## このライブラリについて
PHPの配列に対する操作を、並列で行うクラスです。PHPのwhile, for, foreachなどのループの代わりに使用することを目的としています。

配列の各要素に以下のような処理を行う場合に、処理時間を短く出来る可能性があります。

1. CPU時間のかかる処理を実行する場合
2. DBアクセス、ネットワークアクセスなどブロックされる処理がある場合 

1の場合はマルチコアなCPUでないと効果がありません。

## 動作環境

* PHP5.3以降
* pcntl系の関数が使用できること(mod_phpなどでは動作しません）

PHPをコンパイルするときに --enable-pcntl オプションをつけてください。また、yumやaptでインストールしたPHPでは、最初から使えることが多いようです。

## 仕組み

処理対象の配列をより小さな複数個の配列に分割して、それぞれをpcntl_forkで作成した子プロセスで処理し、結果をマージして返します。分割数は設定で変更できます。


配列の要素に対する処理内容と結果をマージする処理は、それぞれクロージャを作成して渡します。

詳しくはexampleディレクトリとtestディレクトリ内のファイルを見てください。

## 制約

* 実行する前にDB接続、ネットワーク接続、ファイルハンドルなどの全てのリソース型を閉じるようにしてください。これは親プロセスがリソースをオープンしたまま子プロセスを作成し、子プロセス側でこれらを閉じる(スクリプト終了で自動的に閉じられるものも含む)とPHPの挙動が不安定になるからです。プロセスがいきなり終了したりします。

特にフレームワーク内で使用する場合、フレームワークが自動的に開いたDB接続などに注意してください。

# ライセンス

MTI License
