================================================
Asakusa Gradle Plugin バージョン 0.10系 の変更点
================================================

ここでは、Asakusa Gradle Plugin バージョン 0.9系 から 0.10系 の変更点について説明します。

Asakusa Gradle Pluginを利用したアプリケーションプロジェクトのマイグレーションについては、 :doc:`gradle-plugin-migration-guide` も参照してください。

|M3BP_FEATURE| - Hadoopの連携
=============================

|M3BP_FEATURE|\ の :ref:`m3bp-user-guide-using-hadoop` についての標準の動作および設定は、Asakusa Framework バージョン 0.10.0 から以下のように変更されました。

* 過去バージョンでは、標準の設定では実行環境上にインストールしたHadoopと **連携しない** よう設定されていたが、
  バージョン 0.10.0 からは標準の設定で実行環境上にインストールしたHadoopと **連携する** よう変更された。
* 過去のバージョンでは、実行環境上にインストールしたHadoopと連携するために以下の設定を使用したが、
  バージョン 0.10.0 からはこれらの設定が廃止された（設定しても無効となる）。

    * ``asakusafwOrganizer.m3bp.useSystemHadoop``
    * ``asakusafwOrganizer.profiles.<プロファイル名>.m3bp.useSystemHadoop``

* 実行環境のHadoopと連携しない場合、Asakusa Frameworkの実行に必要な最低限のHadoopライブラリーをデプロイメントアーカイブに含めるために、以下の設定が追加された。

    * ``asakusafwOrganizer.hadoop.embed true``

過去のバージョンから移行する場合、上記の変更に基づいてアプリケーションプロジェクトのビルドスクリプト ``build.gradle`` に対して以下の変更を行ってください｡

実行環境上にインストールしたHadoopと連携する場合
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

実行環境上にインストールしたHadoopと連携する場合、過去バージョンの ``build.gradle`` には以下の設定を追加しましたが、
バージョン 0.10.0 以降はこの設定を削除してください。

    * ``asakusafwOrganizer.m3bp.useSystemHadoop``
    * ``asakusafwOrganizer.profiles.<プロファイル名>.m3bp.useSystemHadoop``

実行環境上にインストールしたHadoopと連携しない場合
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

実行環境上にインストールしたHadoopと連携しない場合、 ``build.gradle`` に以下の設定を追加してください。

    * ``asakusafwOrganizer.hadoop.embed true``

以下は、 ``prod`` プロファイルのデプロイメントアーカイブに上記の設定を行う例です。

..  literalinclude:: ../m3bp/attachment/build-hadoop-embed.gradle
    :language: groovy
    :caption: build.gradle
    :name: build.gradle-v010-changes-1
    :emphasize-lines: 19

..  seealso::
    Hadoopとの連携に関する設定については、:doc:`../m3bp/user-guide` - :ref:`m3bp-user-guide-using-hadoop` も参照してください。
