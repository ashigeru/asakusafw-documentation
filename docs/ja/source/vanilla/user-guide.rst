==============================
Asakusa Vanilla ユーザーガイド
==============================

この文書では Asakusa Vanilla の利用方法について説明します。

アプリケーションのテスト
========================

Asakusa Framework バージョン 0.10 以降では、標準の設定でAsakusa Vanillaによるテストが有効になっています。

``./gradlew check`` などによってテストドライバを利用したテストを実行すると、Asakusa Vanillaを利用してバッチアプリケーションを実行します。

..  attention::
    Asakusa Framework バージョン 0.9系ではAsakusa Vanillaを有効にするためにビルドスクリプトに対して以下の設定を行う必要がありましたが、
    バージョン 0.10 以降ではこの定義は不要です。

    ..  code-block:: groovy
        :caption: build.gradle
        :name: build.gradle-asakusa-vanilla-1

        asakusafw {
            sdk.testkit 'vanilla'
        }

    なお、バージョン 0.9系、およびそれ以前に標準のテストドライバとして採用していたスモールジョブ実行エンジンを利用するには、
    ビルドスクリプトに対して以下の設定を行います。
    ただし、バージョン 0.10 以降ではこの設定によるテスト実行は非推奨です。

    ..  code-block:: groovy
        :caption: build.gradle
        :name: build.gradle-asakusa-emulation-1

        asakusafw {
            sdk.testkit 'mapreduce-emulation'
        }

..  note::
    Vanillaを利用したDSLのテストでは、 :file:`$ASAKUSA_HOME/vanilla/conf/vanilla.properties` に記載された設定を利用して Asakusa Vanilla を実行します。
    このファイルが存在しない場合、すべて既定の設定を利用します。

    何らかの設定が必要な場合、該当箇所に設定ファイルを配置してください。
    設定方法については :doc:`reference` - :ref:`vanilla_optimization_properties` を参照してください。

アプリケーションのビルド
------------------------

Asakusa Vanillaは主にテストドライバ上で利用することを想定していますが、DSLコンパイルによってバッチアプリケーションを生成し、これを実行することも可能です。

Asakusa Vanillaを実行エンジンに利用するバッチアプリケーションを生成するには、ディストリビューションプラグイン [#]_ を利用したビルドスクリプト ( :file:`build.gradle` ) に対して以下の設定を追加します。

..  code-block:: groovy
    :caption: build.gradle
    :name: build.gradle-asakusa-vanilla-2

    apply plugin: 'asakusafw-vanilla'

この状態で、Gradleタスク :program:`vanillaCompileBatchapps` を実行すると、Asakusa Vanilla 向けのバッチアプリケーションのビルドを実行します。

..  code-block:: sh

    ./gradlew vanillaCompileBatchapps

:program:`vanillaCompileBatchapps` タスクを実行すると、アプリケーションプロジェクトの :file:`build/vanilla-batchapps` 配下にビルド済みのバッチアプリケーションが生成されます。

標準の設定では、Asakusa Vanillaのバッチアプリケーションは接頭辞に ``vanilla.`` が付与されます。
例えば、サンプルアプリケーションのバッチID ``example.summarizeSales`` の場合、Asakusa VanillaのバッチアプリケーションのバッチIDは ``vanilla.example.summarizeSales`` となります。

..  [#] ディストリビューションプラグインについては、 :doc:`../application/gradle-plugin-reference` を参照してください。

デプロイメントアーカイブの生成
------------------------------

`アプリケーションのビルド`_ の設定を行った状態で、Asakusa Frameworkのデプロイメントアーカイブを作成すると、 Asakusa Vanillaのバッチアプリケーションアーカイブを含むデプロイメントアーカイブを生成します。

デプロイメントアーカイブを生成するには Gradleの :program:`assemble` タスクを実行します。

..  code-block:: sh

    ./gradlew assemble

..  note::
    その他、アプリケーションのデプロイや実行などの方法は他の実行エンジンと同様です。
