==============
リリースノート
==============

Asakusa Frameworkのリリースノートです。
すべての変更点は :doc:`changelogs` を参照してください。

Release 0.9.2
=============

(開発中)

`Asakusa Framework 0.9.2 documentation`_

..  _`Asakusa Framework 0.9.2 documentation`: http://docs.asakusafw.com/0.9.2/release/ja/html/index.html

新機能と主な変更点
------------------

DMDLプロパティ参照
~~~~~~~~~~~~~~~~~~

DMDLプロパティ参照は、DMDLのレコードモデルおよび射影モデルに対して、複数のプロパティを単一のコレクションとして利用する機能をを提供します。

これらは固定長のリスト ( ``java.util.List`` ) またはマップ ( ``java.util.Map`` ) として利用でき、それぞれの要素はデータモデル内に存在するプロパティの参照を表します。 つまり、データモデル中のプロパティの値を変更するとリストやマップの値も変化し、リストやマップに対して変更を行うと、対応するプロパティの値が変化します。

このプロパティ参照を上手に利用すると、データモデル内に存在する「プロパティの繰り返し」を容易に処理できるようになります。

詳しくは、以下のドキュメントを参照してください。

* :doc:`../dmdl/user-guide` - :ref:`dmdl-property-references`

WindGate JDBC ダイレクト・モード を正式機能としてリリース
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

バージョン 0.9.0 から試験的機能として公開していた :ref:`windgate-jdbc-direct-mode` を正式機能として公開しました。

WindGate JDBC ダイレクト・モードとは、WindGateを利用したバッチアプリケーションの実行時にデータフロー処理を行うプロセスの内部で直接
WindGate JDBCによるデータベースへのインポート処理とエクスポート処理を行うように動作する、 |M3BP_FEATURE| 固有の最適化設定です。

WindGate JDBC ダイレクト・モードを利用することで、通常のWindGateよりもバッチアプリケーション全体の実行時間が大きく短縮できる可能性があります。

詳しくは、以下のドキュメントを参照してください。

* :doc:`m3bp/optimization` - :ref:`windgate-jdbc-direct-mode`

WindGate - Oracleパーティションテーブルの並列読み出し
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

WindGateの最適化オプションに、Oracleパーティションテーブルに対する並列読み出しを行うオプションが追加されました。

この機能は、 :ref:`windgate-jdbc-direct-mode` でのみ利用することができます。
この機能の設定方法などは、 :ref:`windgate-jdbc-direct-mode` のドキュメントを参照してください。

その他の変更点
--------------

* :doc:`spark/index` - Direct I/O のファイル出力処理中に処理が失敗しても正常終了として扱われることがある問題を修正
* :doc:`spark/index` - :ref:`summarize-operator` の 集約関数 ``max`` を適用するデータに ``null`` が含まれていても、エラーにならず正常終了として扱われることがある問題を修正
* :doc:`spark/optimization` に Direct I/Oの入力スプリットを結合するオプション ``spark.hadoop.com.asakusafw.bridge.directio.input.combine`` を追加
* :doc:`m3bp/optimization` に :ref:`spill-input-buffer` で使用するディレクトリを変更するオプション ``com.asakusafw.dag.input.file.directory`` を追加

その他、細かな機能改善およびバグフィックスが含まれます。
すべての変更点は :doc:`changelogs` を参照してください。

Release 0.9.1
=============

Apr 26, 2017

`Asakusa Framework 0.9.1 documentation`_

..  _`Asakusa Framework 0.9.1 documentation`: http://docs.asakusafw.com/0.9.1/release/ja/html/index.html

新機能と主な変更点
------------------

Direct I/O formatted text
~~~~~~~~~~~~~~~~~~~~~~~~~

Direct I/Oでレコードやフィールドを区切り文字によって分割するデータ形式を取り扱うための機能 :doc:`../directio/formatted-text` を追加しました。

従来、Direct I/Oで提供していた :doc:`../directio/csv-format` や :doc:`../sandbox/directio-tsv` などのテキストフォーマットを扱う機能と比べて、以下のような特徴を持っています。

* 多様なデータ形式を扱うきめ細かな設定が可能

  * データ形式に関する様々な設定をレコード全体、及びフィールド単位にそれぞれ設定することができます。
  * 従来、事前にデータ整形処理が必要であったようなデータ形式の多くは、この機能で直接処理可能となります。
* 不整合データに対する柔軟な動作設定が可能

  * 不整合データの検出時にエラーとして異常終了、警告を出力して処理を継続、単にスキップするなど様々な動作設定が可能です。
  * 想定外のデータやアプリケーションの仕様変更などに対して柔軟な対応が可能となります。

詳しくは、以下のドキュメントを参照してください。

* :doc:`../directio/formatted-text`

Direct I/Oのデータフォーマットファイルをテストデータに指定
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Direct I/Oを利用したアプリケーションのテストでは通常のテスト方法に加えて、Direct I/Oのデータフォーマットに対応するファイルをテストデータとして指定することが可能になりました。

例えば、CSVやTSVファイルを演算子やデータフローのテストに対する入力データや期待値データとして指定することができます。

詳しくは、以下のドキュメントを参照してください。

* :doc:`../directio/user-guide` - :ref:`directio-testdriver-dataformat`

その他の変更点
--------------

* Direct I/O の :ref:`directio-output-filename-pattern` で利用可能なプレースホルダで数値書式の対応を追加
* :ref:`operator-testing-with-result` 向けのユーティリティメソッド ``OperatorTestEnvironment#newResult`` を追加
* :ref:`vup-gradle-wrapper` の手順で設定されるGradleラッパーの取得先URLをGradleの標準設定に合わせて ``http`` から ``https`` に変更
* :doc:`../m3bp/index` 標準の設定下で単一の入力グループが2GB以上になるとエラーが発生する制限事項を解消
* :doc:`../m3bp/index` 特定の条件でデータフローのコンパイルが正しく行われないことがある問題 [#]_ を修正
* :ref:`windgate-jdbc-direct-mode` 特定の条件でデッドロックが発生する問題を修正
* :doc:`sandbox/intellij-idea` (試験的機能) ドキュメントの追加、及び従来の利用手順(Gradle IDEA Plugin拡張の利用)を非推奨化
* :doc:`product/target-platform` のアップデート

その他、細かな機能改善およびバグフィックスが含まれます。
すべての変更点は :doc:`changelogs` を参照してください。

..  [#] https://github.com/asakusafw/asakusafw-compiler/pull/119

互換性に関して
--------------

本リリースでは過去バージョンとの互換性に関する特別な情報はありません。

Release 0.9.0
=============

Dec 05, 2016

`Asakusa Framework 0.9.0 documentation`_

..  _`Asakusa Framework 0.9.0 documentation`: http://docs.asakusafw.com/0.9.0/release/ja/html/index.html

はじめに
--------

今回のリリースよりAsakusa Frameworkのリリース方式を変更し、
Asakusa Frameworkのコアや拡張コンポーネント群をまとめた「ディストリビューション」形式でリリースを行います。

従来のリリース方式ではAsakusa Frameworkが提供する各コンポーネントは個別にリリースされていました。
このためアプリケーション開発者は利用するコンポーネントのバージョンを個別に把握して設定する必要がありました。
またこれにより、互換性がないコンポーネントバージョンを組み合わせて設定してしまう恐れがありました。

今回採用するディストリビューション形式のリリースにより、
Asakusa Frameworkが提供する様々なコンポーネントを単一のバージョンで利用可能にします。
ディストリビューションに含まれる各コンポーネントの組み合わせは互換性があることが保証されます。

今回のリリース以降、このドキュメントはディストリビューションのリリース単位で作成されます。
このドキュメントに記載するAsakusa Frameworkのバージョンは、特別な記載がない限りディストリビューションのバージョンを表します。

ディストリビューションに含まれる各コンポーネントとそのバージョンは :doc:`changelogs` に記載しています。

新機能と主な変更点
------------------

Asakusa Framework Core/SDK
~~~~~~~~~~~~~~~~~~~~~~~~~~

Asakusa Gradle Plugin - ディストリビューションプラグイン
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

`はじめに`_ で説明したディストリビューションを利用するために、
Asakusa Gradle Pluginに「ディストリビューションプラグイン」を追加しました。
Asakusa Framework 0.9.0 以降のプロジェクトテンプレートを利用する場合、標準でこのディストリビューションプラグインが使用されます。

バージョン 0.8系を利用しているプロジェクトからのマイグレーションについては、以下のドキュメントを参照してください。

* :doc:`application/gradle-plugin-v09-changes`

WindGate - データベース接続時の最適化オプション
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

WindGateのデータベースとの接続時に利用する最適化オプションを設定する機能が追加されました。
この機能は主にデータベース固有の最適化機能を利用するために使用することを想定しています。

Asakusa Framework 0.9.0 では Oracleのダイレクト・パス・インサートを利用するクエリーを発行するためのオプションが追加されています。

詳しくは、以下のドキュメントを参照してください。

* :doc:`windgate/user-guide` - :ref:`windgate-resource-jdbc-optimizations`

Direct I/O - CSVフィールドのクォート方式の指定
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Direct I/O CSVでフィールド値の内容に関わらず、常にクォート処理を行うモードが追加されました。
また、CSVの各フィールドで個別にモードを指定するためのDMDL記述が追加されました。

Direct I/O CSVと連携するプロダクトでCSVフォーマットのクォートに関する制約がある場合に、この機能を利用することで連携時の問題を解消できる可能性があります。

詳しくは、以下のドキュメントを参照してください。

* :doc:`directio/csv-format` - :ref:`directio-csv-field-settings`

Asakusa on Spark
~~~~~~~~~~~~~~~~

Direct I/Oの出力処理をSpark上で実行
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

これまでのバージョンでは、Asakusa on Sparkを利用する場合でもDirect I/Oの出力処理はMapReduceジョブによって実行されていましたが、
本バージョンより、Direct I/Oの出力処理はSpark上で実行されるようになりました。

この改善により、Asakusa on Spark上でDirect I/Oを利用するバッチアプリケーションについては、Hadoop MapReduceを実行するための環境設定は不要になりました。
またMapReduceジョブの実行に必要なオーバーヘッドがなくなることで、バッチアプリケーションの実行性能が向上する可能性があります。

なお、この動作はコンパイラオプション ``spark.output.direct`` により変更可能です。詳しくは、以下のドキュメントを参照してください。

* :doc:`spark/reference`

またこの改善により、 :doc:`spark/iterative-extension` ではDirect I/Oの出力処理でも反復変数が利用可能になりました。

Spark 2.0に対応
^^^^^^^^^^^^^^^

本リリースより Spark 2.0 以降のバージョンに対応しました。

なお、本リリースより Spark 1.6 以前のバージョンは非対応になりました。

|M3BP_FEATURE|
~~~~~~~~~~~~~~~~

WindGate JDBC ダイレクト・モード (試験的機能)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

WindGate JDBC ダイレクト・モードとは、WindGateを利用したバッチアプリケーションの実行時にデータフロー処理を行うプロセスの内部で直接
WindGate JDBCによるデータベースへのインポート処理とエクスポート処理を行うように動作する、 |M3BP_FEATURE| 固有の最適化設定です。

WindGate JDBC ダイレクト・モードを利用することで、通常のWindGateよりもバッチアプリケーション全体の実行時間が大きく短縮できる可能性があります。

詳しくは、以下のドキュメントを参照してください。

* :doc:`m3bp/optimization` - :ref:`windgate-jdbc-direct-mode`

その他
~~~~~~

Asakusa Vanilla (試験的機能)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Asakusa Vanillaは、Asakusa Frameworkの内部機能として提供するDAG実行エンジン実装用のツールセットを使った、実行エンジンのリファレンス実装です。

Asakusa Vanillaは単一ノード上でアプリケーションを実行します。
軽量で比較的コンパイル速度が速く、実行時にJVM以外の環境を必要としない、といった特徴を持っています。
このため将来のバージョンでは、Asakusa Vanillaを :doc:`testing/emulation-mode` で利用する標準の実行エンジンに採用することを計画しています。

詳しくは、以下のドキュメントを参照してください。

* :doc:`sandbox/asakusa-vanilla`

互換性に関して
--------------

対応プラットフォーム
~~~~~~~~~~~~~~~~~~~~

本リリースでは、対応プラットフォームに関する重要な変更と非互換性があります。

Java (JDK)
  Java7、およびJDK 7は非対応になりました。

  Java7、およびJDK 7を利用している場合、Java 8 (JDK 8)に移行する必要があります。

Spark
  Spark 1.6 以前のバージョンは非対応になりました。

  Spark 1.6、およびそれ以前のバージョンを利用している場合、Spark 2.0 以降のバージョンに移行する必要があります。

変更内容の詳細やマイグレーション手順については、以下のドキュメント説明しています。

* :doc:`application/migration-guide`
* :doc:`administration/migration-guide`

削除された機能
~~~~~~~~~~~~~~

本リリースより、以下の機能は削除されました。

* レガシーモジュール
* Asakusa Legacy Gradle Plugin
* YAESSログの可視化 ( ``summarizeYaessJob`` タスク )

リンク
======

* :doc:`previous-release-notes`
* :doc:`changelogs`

