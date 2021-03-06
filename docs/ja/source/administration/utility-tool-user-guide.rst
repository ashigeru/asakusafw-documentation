==================================
ユーティリティツールユーザーガイド
==================================

この文書では、Asakusa Frameworkが提供するコマンドラインツールの利用方法について解説します。
このコマンドラインツール群を利用することで、運用環境上のメンテナンス作業を軽減します。

ユーティリティツールの配置
==========================

すべてのユーティリティツールは、Asakusa Frameworkをインストールしたパスの :file:`$ASAKUSA_HOME/tools/bin` ディレクトリ内に格納されています。

ユーティリティツールのコマンド一覧を以下に示します。

..  list-table:: ユーティリティツール コマンド一覧
    :widths: 2 8
    :header-rows: 1

    * - サブコマンド
      - 説明
    * - :program:`directio`
      - Direct I/O CLI [#]_
    * - :program:`directio.cmd`
      - Direct I/O CLI のWindows向けコマンド
    * - :file:`setup.jar`
      - `デプロイメントアーカイブのセットアップ`_
    * - :program:`hadoop-fs-clean.sh`
      - `Hadoopファイルシステムのクリーニング`_

..  [#] Direct I/O CLI については :doc:`../directio/directio-cli` を参照してください。

セットアップに関するユーティリティ
==================================

デプロイメントアーカイブのセットアップ
--------------------------------------

:file:`setup.jar` はAsakusa Frameworkのデプロイメントアーカイブを展開後に実行することで
展開したファイルに対して適切な実行権限を付与するなどの処理を行い、バッチアプリケーションを実行可能な状態にセットアップします。

利用方法の例は、 :doc:`../introduction/start-guide` や :doc:`../administration/deployment-guide` を参照してください。

書式
~~~~

..  code-block:: sh

    java -jar $ASAKUSA_HOME/tools/bin/setup.jar [/path/to/installation] [--help]

``setup.jar`` は :program:`java` コマンドを経由して実行します。
オプションを指定せず実行した場合、 環境変数 ``$ASAKUSA_HOME`` 配下に配置されているファイルに対してセットアップ処理を行います。
オプションにディレクトリパスを指定すると、そのディレクトリ配下に配置されているファイルに対してセットアップ処理を行います。

使用例
~~~~~~

..  code-block:: sh

    mkdir -p "$ASAKUSA_HOME"
    cd "$ASAKUSA_HOME"
    tar -xzf /tmp/asakusafw-example-basic-m3bp.tar.gz
    java -jar $ASAKUSA_HOME/tools/bin/setup.jar

Hadoopに関するユーティリティ
============================

Hadoopに関するユーティリティツールは、 :file:`$ASAKUSA_HOME/tools/bin` ディレクトリ内の :file:`hadoop-` から始まるスクリプトとして提供しています。

いずれのツールも :program:`hadoop` コマンドを経由して実行するため、環境にHadoopがインストールされている必要があります。
また、Hadoopのインストール先を通知するために、以下の環境変数のいずれかが必要です。

``HADOOP_CMD``
  ``hadoop`` コマンドのパス
``HADOOP_HOME``
  Hadoopのインストール先
``PATH``
  :program:`hadoop` コマンドが通っているパス

環境変数の設定方法は、 `コマンドラインツール全体の設定`_ を参照してください。

Hadoopファイルシステムのクリーニング
------------------------------------

:program:`$ASAKUSA_HOME/tools/bin/hadoop-fs-clean.sh` コマンドを利用すると、Hadoopファイルシステム上の古いファイルやディレクトリを一括して削除できます。

以下の形式で指定します。

..  code-block:: sh

    hadoop-fs-clean.sh -k <days> [-r] [-s] <path> [<path> [...]]

コマンドに指定可能な引数は以下のとおりです。

..  program:: hadoop-fs-clean.sh

..  option:: -h , -help

    ヘルプメッセージを表示して終了します。

..  option:: -k <days> , -keep-days <days> (必須)

    最終更新から ``<days>`` 日以上経過したファイルのみを削除します。
    ``0`` を指定した場合には現在時刻よりも古いファイルをすべて削除します。

..  option:: -r , -recursive

    ディレクトリとその内容を再帰的にクリーニングの対象とします。
    クリーニングによってディレクトリの中身が空になった場合、ディレクトリも削除の対象になります。

..  option:: -s , -dry-run

    クリーニング時にファイルやディレクトリの削除を行わず、ログだけを出力します。

..  option:: path (必須)

    クリーニング対象のパスをURI形式で指定します。
    2つ以上のパスを指定することもできます。

    ``*`` を含むパスなど、 :program:`hadoop fs` コマンドで有効なパス式を指定できます。

    なお、 コマンド引数に ``--`` を指定すると以降の引数をすべて ``<path>`` とみなします。
    対象のURIが ``-`` から始まる場合などに有効です。

以下は利用例です。

..  code-block:: sh

    # HDFS上の /user/asakusa/var/logs ディレクトリ直下のうち、1日経過したファイルを削除する
    hadoop-fs-clean.sh -k 1 hdfs://localhost:8020/user/asakusa/var/logs/*

    # HDFS上の /user/asakusa/var/logs ディレクトリ内の、10日経過したファイルやディレクトリを再帰的に削除する
    hadoop-fs-clean.sh -k 10 hdfs://localhost:8020/user/asakusa/var/logs/* -r

    # ローカルファイルシステム上の /tmp/hadoop-asakusa ディレクトリに対する全削除をシミュレーションする
    hadoop-fs-clean.sh -dry-run -k 0 file:///tmp/hadoop-asakusa -r

コマンドラインツール全体の設定
==============================

上記で紹介したコマンドラインツールは、実行前に :file:`$ASAKUSA_HOME/tools/env.sh` を読み込んで必要な環境変数の設定などを行います。

以下は同ファイルの内容を改変し、環境変数 ``HADOOP_CMD`` を設定する例です。

..  code-block:: sh

    export HADOOP_CMD=/usr/bin/hadoop

コマンドラインツールのログ設定
==============================

上記で紹介したコマンドラインツールは、Hadoopが持つLog4Jのルートロガーに対して出力を行います。

多くのHadoopディストリビューションでは、デフォルトではLog4Jのルートロガーは標準エラー出力に対してログ出力を行うようになっています。

