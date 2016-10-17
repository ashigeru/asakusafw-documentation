=================================
Asakusa on MapReduce リファレンス
=================================

この文書では、Asakusa on MapReduceが提供するGradle PluginやDSLコンパイラの設定、およびバッチアプリケーション実行時の設定などについて説明します。

.. _mapreduce-compile-options:

Asakusa on MapReduce Gradle Plugin リファレンス
===============================================

Asakusa on MapReduce Gradle Pluginが提供する機能とインターフェースについて個々に解説します。

プラグイン
----------

``asakusafw-mapreduce``
    アプリケーションプロジェクトで、Asakusa on MapReduceのさまざまな機能を有効にする [#]_ 。

    このプラグインは ``asakusafw`` プラグインや ``asakusafw-organizer`` プラグインを拡張するように作られているため、それぞれのプラグインも併せて有効にする必要がある（ ``apply plugin: 'asakusafw-mapreduce'`` だけではほとんどの機能を利用できません）。

..  [#] :asakusa-gradle-groovydoc:`com.asakusafw.mapreduce.gradle.plugins.AsakusafwMapReducePlugin`

タスク
------

``mapreduceCompileBatchapps``
    Asakusa DSL Compiler for MapReduceを利用してDSLをコンパイルする [#]_ 。

    ``asakusafw`` プラグインが有効である場合にのみ利用可能。

``attachComponentMapReduce``
    デプロイメントアーカイブにMapReduce向けのバッチアプリケーションを実行するためのコンポーネントを追加する。

    ``asakusafw-organizer`` プラグインが有効である場合にのみ利用可能。

    ``asakusafwOrganizer.mapreduce.enabled`` に ``true`` が指定されている場合、自動的に有効になる。

``attachMapReduceBatchapps``
    デプロイメントアーカイブに ``mapreduceCompileBatchapps`` でコンパイルした結果を含める。

    ``asakusafw`` , ``asakusafw-organizer`` の両プラグインがいずれも有効である場合にのみ利用可能。

    ``asakusafwOrganizer.batchapps.enabled`` に ``true`` が指定されている場合、自動的に有効になる。

..  [#] :asakusa-gradle-groovydoc:`com.asakusafw.gradle.tasks.AsakusaCompileTask`

タスク拡張
----------

``assemble``
    デプロイメントアーカイブを生成する。

    ``asakuafw-mapreduce`` と ``asakusafw-organizer`` プラグインがいずれも有効である場合、 ``mapreduceCompileBatchapps`` が依存関係に追加される。

``compileBatchapp``
    Asakusa DSLコンパイラを使ってバッチアプリケーションのコンパイルを行い、実行可能モジュールを生成する。

    ``asakuafw-mapreduce`` プラグインが有効である場合、 ``mapreduceCompileBatchapps`` が依存関係に追加される。

``jarBatchapp``
    ``compileBatchapp`` タスクで生成したバッチアプリケーションを含むjarファイルを生成する。

    ``asakuafw-mapreduce`` プラグインが有効である場合、 ``mapreduceCompileBatchapps`` タスクの生成物がjarファイルの内容に追加される。

規約プロパティ拡張
------------------

.. _mapreduce-batch-application-plugin-ext:

Batch Application Plugin ( ``asakusafw`` ) への拡張
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Asakusa on MapReduce Gradle Pluginは Batch Application Plugin に対して Asakusa on MapReduceのビルド設定を行うための規約プロパティを追加します。この規約プロパティは、 ``asakusafw`` ブロック内の参照名 ``mapreduce`` でアクセスできます [#]_ 。

以下、 ``build.gradle`` の設定例です。

..  code-block:: groovy
    :caption: build.gradle
    :name: build.gradle-mapreduce-reference-1

    asakusafw {
        mapreduce {
            include 'com.example.batch.*'
            option 'hashJoinForTiny', 'false'
        }

この規約オブジェクトは以下のプロパティを持ちます。

``mapreduce.outputDirectory``
    コンパイラの出力先を指定する。

    文字列や ``java.io.File`` などで指定し、相対パスが指定された場合にはプロジェクトからの相対パスとして取り扱う。

    既定値: ``"$buildDir/batchc"``

``mapreduce.include``
    コンパイルの対象に含めるバッチクラス名のパターンを指定する。

    バッチクラス名には ``*`` でワイルドカードを含めることが可能。

    また、バッチクラス名のリストを指定した場合、それらのパターンのいずれかにマッチしたバッチクラスのみをコンパイルの対象に含める。

    既定値: ``null`` (すべて)

``mapreduce.exclude``
    コンパイルの対象から除外するバッチクラス名のパターンを指定する。

    バッチクラス名には ``*`` でワイルドカードを含めることが可能。

    また、バッチクラス名のリストを指定した場合、それらのパターンのいずれかにマッチしたバッチクラスをコンパイルの対象から除外する。

    ``include`` と ``exclude`` がいずれも指定された場合、 ``exclude`` のパターンを優先して取り扱う。

    既定値: ``null`` (除外しない)

``mapreduce.runtimeWorkingDirectory``
    実行時のテンポラリワーキングディレクトリのパスを指定する。

    パスにはURIやカレントワーキングディレクトリからの相対パスを指定可能。

    未指定の場合、コンパイラの標準設定である「 ``target/hadoopwork`` 」を利用する。

    既定値: ``null`` (コンパイラの標準設定を利用する)

``mapreduce.option``
    `コンパイラプロパティ`_ （コンパイラのオプション設定）を追加する。

    後述する `コンパイラプロパティ`_ を ``<key>, <value>`` の形式で指定する [#]_ 。

    既定値: (MapReduce向けのコンパイルに必要な最低限のもの)

``mapreduce.failOnError``
    MapReduce向けのコンパイルを行う際に、コンパイルエラーが発生したら即座にコンパイルを停止するかどうかを選択する。

    コンパイルエラーが発生した際に、 ``true`` を指定した場合にはコンパイルをすぐに停止し、 ``false`` を指定した場合には最後までコンパイルを実施する。

    既定値: ``true`` (即座にコンパイルを停止する)

..  [#] これらのプロパティは規約オブジェクト :asakusa-gradle-groovydoc:`com.asakusafw.gradle.plugins.AsakusafwCompilerExtension` が提供します。
..  [#] コンパイラプロパティを指定する方法は他にいくつかの方法があります。詳しくは :asakusa-gradle-groovydoc:`com.asakusafw.gradle.plugins.AsakusafwCompilerExtension` のメソッドの説明を参照してください。

.. _mapreduce-framework-organizer-plugin-ext:

Framework Organizer Plugin ( ``asakusafwOrganizer`` ) への拡張
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Asakusa on MapReduce Gradle Plugin は Framework Organizer Plugin に対して Asakusa on MapReduceのビルド設定を行うための規約プロパティを追加します。この規約プロパティは、 ``asakusafwOrganizer`` ブロック内の参照名 ``mapreduce`` でアクセスできます [#]_ 。

この規約オブジェクトは以下のプロパティを持ちます。

``mapreduce.enabled``
    デプロイメントアーカイブにMapReduce向けのバッチアプリケーションを実行するためのコンポーネントを追加するかどうかを指定する (各プロファイルのデフォルト値)。

    ``true`` を指定した場合にはコンポーネントを追加し、 ``false`` を指定した場合には追加しない。

    既定値: ``true`` (コンポーネントを追加する)

``<profile>.mapreduce.enabled``
    対象のプロファイルに対し、デプロイメントアーカイブにMapReduce向けのバッチアプリケーションを実行するためのコンポーネントを追加するかどうかを指定する。

    前述の ``mapreduce.enabled`` と同様だが、こちらはプロファイルごとに指定できる。

    既定値: ``asakusafwOrganizer.mapreduce.enabled`` (全体のデフォルト値を利用する)

..  [#] これらのプロパティは規約オブジェクト :asakusa-gradle-groovydoc:`com.asakusafw.mapreduce.gradle.plugins.AsakusafwOrganizerMapReduceExtension` が提供します。

コマンドラインオプション
------------------------

:program:`mapreduceCompileBatchapps` タスクを指定して :program:`gradlew` コマンドを実行する際に、 ``mapreduceCompileBatchapps --update <バッチクラス名>`` と指定することで、指定したバッチクラス名のみをバッチコンパイルすることができます。

また、バッチクラス名の文字列には ``*`` をワイルドカードとして使用することもできます。

以下の例では、パッケージ名に ``com.example.target.batch`` を含むバッチクラスのみをバッチコンパイルしてデプロイメントアーカイブを作成しています。

..  code-block:: sh

    ./gradlew mapreduceCompileBatchapps --update com.example.target.batch.* assemble

そのほか、 :program:`mapreduceCompileBatchapps` タスクは :program:`gradlew` コマンド実行時に以下のコマンドライン引数を指定することができます。

..  program:: mapreduceCompileBatchapps

..  option:: --options <k1=v1[,k2=v2[,...]]>

    追加のコンパイラプロパティを指定する。

    規約プロパティ ``asakusafw.mapreduce.option`` で設定したものと同じキーを指定した場合、それらを上書きする。

..  option:: --fail-on-error <"true"|"false">

    コンパイルエラー発生時に即座にコンパイル処理を停止するかどうか。

    規約プロパティ ``asakusafw.mapreduce.failOnError`` の設定を上書きする。

..  option:: --update <batch-class-name-pattern>

    指定のバッチクラスだけをコンパイルする (指定したもの以外はそのまま残る)。

    規約プロパティ ``asakusafw.mapreduce.{in,ex}clude`` と同様にワイルドカードを利用可能。

    このオプションが設定された場合、規約プロパティ ``asakusafw.mapreduce.{in,ex}clude`` の設定は無視する。

.. _mapreduce-dsl-compiler-reference:

Asakusa DSL Compiler for MapReduce リファレンス
===============================================

コンパイラプロパティ
--------------------

Asakusa DSL Compiler for MapReduceで利用可能なコンパイラプロパティについて説明します。これらの設定方法については、 `Batch Application Plugin ( asakusafw ) への拡張`_ の ``mapreduce.option`` の項を参照してください。

..  list-table:: Asakusa DSL Compiler for MapReduce - コンパイルオプション
    :widths: 2 1 7
    :header-rows: 1

    * - 項目名
      - 既定値
      - 概要
    * - ``enableCombiner``
      - 無効
      - 部分集約 [#]_ の既定値。

        部分集約を許す演算子に対して ``PartialAggregation.DEFAULT`` が [#]_ 指定された場合に、このオプションが有効であれば部分集約を行い、そうでなければ行わない。
    * - ``compressFlowPart``
      - 有効
      - ステージ数が少なくなる方法でフロー演算子を展開する。

        このオプションが無効であればフロー演算子の展開時に全ての入出力にチェックポイント演算子を挿入する。
        このオプションが有効であれば、展開時に何も挿入しない。
    * - ``compressConcurrentStage``
      - 有効
      - 互いに影響のないステージを1つのステージに合成する。

        このオプションが有効であれば、互いに依存関係のない2つ以上のステージを単一のステージに合成し、無効であれば合成しない。
    * - ``hashJoinForTiny``
      - 有効
      - データサイズに ``DataSize.TINY`` と指定したジョブフローの入力をマスタとして結合する際に、可能であればハッシュ表での結合を行う。

        このオプションが有効であれば上記の動作を行い、無効であればコンパイラが自動的に結合戦略を決定する。
    * - ``hashJoinForSmall``
      - 無効
      - 将来の拡張のためにリザーブされた項目。現在は動作に影響しない。
    * - ``enableDebugLogging``
      - 無効
      - ``Logging.Level.DEBUG`` が指定されたロギング演算子を利用可能にする。

        このオプションが有効であれば、そのようなロギング演算子をコンパイル後も保持する。
        無効であれば、コンパイル時にそれらの演算子を除去する。

上記の他に、 ``X`` から始まるいくつかの `コンパイラスイッチ`_ も存在します。
コンパイラスイッチもコンパイルオプションと同じシステムプロパティを利用します。

..  note::
    ``compressFlowPart`` の既定値は0.2から「有効」に変更しました。
    チェックポイント演算子はMapReduceの単位 (ステージ) に区切りをいれる演算子で、元は「フロー部品のテスト時とできるだけ同じ構造にしたほうが良い」という前提でこのオプションを無効化していました。
    しかし、あまりにMapReduceの回数が増えてしまい、処理効率が著しく低下するため、0.2よりこの規定値が見直されることになりました。

..  note::
    ``compressConcurrentStage`` は利点と欠点のある最適化です。
    この最適化により、ステージ数は最小で「クリティカルパスのステージ数」まで低下します。
    しかし、ここで合成されるステージは本来互いに影響がありませんので、Hadoopはこれらのステージを同時に処理することが可能です。

    この最適化の欠点は、時間のかかるステージとかからないステージを合成してしまうと、後者のステージが本来先に終わる場合でも、前者のステージの処理が完了するまで余計な待ち合わせが発生してしまう点です。
    Hadoopクラスターが十分に大きく、ワークフローエンジンが並列のジョブ投入をサポートしている場合は、このオプションは見直すべきでしょう。

..  note::
    ``hashJoinForTiny`` は、Hadoopの *DistributedCache* の仕組みを利用しています。
    ハッシュ表での結合を行う場合、入力データをHadoopクラスターの全てのノードに配布します。
    そこでハッシュ表を構築し、タスクのメモリ上に保持します。

    現在の標準的な結合戦略はShuffle+Sortを利用したマージ結合であるため、これは結合操作を行うたびにReduceフェーズが必要になってしまいます。
    結果としてMapReduceのステージ数が増大してしまいますが、ハッシュ表を利用する場合には全てのノードのメモリ上に表を構築しているため、Reduce処理が不要になり、ステージ数を削減できるという利点があります。

    ただし、およそハッシュ表の元になったデータサイズの倍程度のメモリを必要とするため、適用範囲が限られてしまうという問題はあります。

..  [#] 部分集約の設定については、 :doc:`../dsl/operators` の単純集計演算子や畳み込み演算子を参照してください。
..  [#] :asakusafw-javadoc:`com.asakusafw.vocabulary.flow.processor.PartialAggregation`

コンパイラスイッチ
~~~~~~~~~~~~~~~~~~

コンパイラスイッチはコンパイラの内部的な挙動を操作するためのオプションで、 `コンパイラプロパティ`_ と同様の方法で設定します。

..  hint::
    通常の場合、コンパイラスイッチを指定する必要はありません。
    コンパイル時にコンパイラから推奨される場合がありますので、その際に利用を検討してください。

すべてのコンパイラスイッチは ``X<項目名>=<値>`` の形式で設定します。
以下は変更可能なコンパイラスイッチの一覧です。

..  list-table:: コンパイラスイッチの項目
    :widths: 2 1 7
    :header-rows: 1

    * - 項目名
      - 既定値
      - 概要
    * - ``MAPREDUCE-370``
      - ``DISABLED``
      - 利用中のHadoopにパッチ ``MAPREDUCE-370`` が適用済みかどうか。
        ``ENABLED`` の場合は適用済みと仮定し、 ``DISABLED`` の場合は未適用と仮定する。
    * - ``compressFlowBlockGroup``
      - ``ENABLED``
      - `コンパイラプロパティ`_ の ``compressConcurrentStage`` を適用した際、ステージ内のMapperとReducerを併合するかどうか。
        ``ENABLED`` の場合は併合し、 ``DISABLED`` の場合は併合しない。
    * - ``packaging``
      - ``ENABLED``
      - アプリケーションのパッケージングを行うかどうか。
    * - ``javaVersion``
      - ``1.7`` [#]_
      - DSLコンパイラがコンパイル時に指定するJavaのバージョン

..  tip::
    コンパイルオプションは項目名を間違えた場合にエラーとなりますが、コンパイラスイッチは項目名を間違えると単に設定が無視されます。

..  [#] :doc:`../application/gradle-plugin` に従ってアプリケーションプロジェクトを作成した場合は、Gradle Pluginの設定値が適用されます。詳しくは :doc:`../application/gradle-plugin-reference` を参照してください。
