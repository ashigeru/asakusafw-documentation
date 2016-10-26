====================================
過去の開発環境マイグレーションガイド
====================================
この文書では、Asakusa Framework のバージョンアップに伴う、開発環境のマイグレーション手順について解説します。

Gradleプロジェクトのマイグレーション手順
========================================
:doc:`../introduction/start-guide` や :doc:`gradle-plugin` の手順に従って構築したアプリケーションプロジェクトについては、まず以下のドキュメントを参照してマイグレーションを実施してください。

* :ref:`vup-gradle-plugin`

上記ドキュメントの手順に加えて、バージョンによっては
後述するバージョン固有のマイグレーション手順が必要となる場合があるので、
必ず以下の内容を確認してください。

なお、複数バージョンをまたいだマイグレーションを行う場合は中間のバージョンの手順も確認し、パッチ適用手順などが提供されていた場合は、必ずそのパッチを順次適用するようにしてください。

バージョン 0.8.1 へのマイグレーション
-------------------------------------

Asakusa Frameworkバージョン指定の非推奨化
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

バージョン 0.8.1 より、プロジェクトのビルドスクリプト ( :file:`build.gradle` ) でAsakusa Frameworkバージョンを指定することは非推奨となりました。

バージョン 0.7.6 より以前では、プロジェクトテンプレートに含まれるビルドスクリプトにAsakusa Frameworkバージョンの指定が含まれていました。
このため特にバージョン 0.7.6 以前に作成したプロジェクトについては、:doc:`gradle-plugin-v08-changes` - :ref:`gradle-plugin-v08-specify-asakusafw-version`
の説明を確認の上、ビルドスクリプトからAsakusa Frameworkバージョンの指定を削除するよう変更してください。

なお本バージョンより、ビルドスクリプトにAsakusa Frameworkバージョンの指定が含まれている場合にはビルド時に警告が表示されるようになりました。

Direct I/O lineの利用方法
~~~~~~~~~~~~~~~~~~~~~~~~~

:doc:`Direct I/O line <../directio/directio-line>` が試験的機能から正式機能に変更になったことに伴い、
Direct I/O lineは :doc:`SDKアーティファクト <sdk-artifact>` ``asakusa-sdk-directio`` に含まれるようになりました。
プロジェクトテンプレートから作成したプロジェクトでは、このライブラリは標準で利用可能になっています。

バージョン 0.8.0 以前では、このライブラリは ``com.asakusafw.sandbox:asakusa-directio-dmdl-ext`` に含まれていました。
このため、Direct I/O lineを利用するのみの目的でこのライブラリをビルドスクリプトに追加している場合、この定義は削除することができます。

バージョン 0.8.0 へのマイグレーション
-------------------------------------

..  warning::
    バージョン 0.8.0 は以前のバージョンからいくつかの重要な変更が行われました。
    マイグレーションを検討する際には必ず以下の内容を確認してください。

Java (JDK)
~~~~~~~~~~

Java6、およびJDK 6は非対応になりました。

Java6、およびJDK 6を利用している場合、Java 7(JDK 7)、またはJava8 (JDK 8)に移行する必要があります。

開発環境のJavaに関する設定については、以下のドキュメントを参照してください。

* :doc:`using-jdk`

Hadoop
~~~~~~

Hadoop1系は非対応となりました。

開発環境にHadoop1系をインストールしている場合、Hadoop2系をインストールしてAsakusa FrameworkからはHadoop2系を利用するよう設定してください。

開発環境にHadoop2系をインストールする方法については、以下のドキュメントを参照してください。

* :doc:`using-hadoop`

Gradle
~~~~~~

Gradleのバージョン1系は非対応になりました。

また、Asakusa Gradle Pluginにいくつか仕様変更が行われ、一部のタスクの動作やビルドスクリプトの設定方法が変更されています。

具体的なアプリケーションプロジェクトのマイグレーション手順については、以下のドキュメンを参照してください。

* :doc:`gradle-plugin-migration-guide` - :ref:`vup-gradle-plugin`

また、Asakusa Gradle Pluginの動作に関する変更内容や、Asakusa Framework バージョン 0.6系, 0.7系 からのビルドスクリプトの移行方法については、以下のドキュメントを参照してください。

* :doc:`gradle-plugin-v08-changes`

..  attention::
    :doc:`gradle-plugin-v08-changes` には Asakusa Frameworkのバージョン体系の変更などの重要な内容が説明されているため、マイグレーション時には必ずこのドキュメントを確認してください。

Maven
~~~~~

Mavenの利用は非対応になりました。

Mavenを利用しているアプリケーションプロジェクトは、Gradleを利用するよう移行する必要があります。

Mavenベースのアプリケーションプロジェクトについては、以下のドキュメンを参照してGradleプロジェクトへのマイグレーションを実施してください。

* :doc:`gradle-plugin` - :ref:`migrate-from-maven-to-gradle`

バージョン 0.7.1 〜 0.7.6 へのマイグレーション
----------------------------------------------

バージョン 0.7.1 から 0.7.6 におけるバージョン固有のマイグレーション手順はありません。
各バージョン共通のマイグレーション手順のみを実施してください。

バージョン 0.7.0 へのマイグレーション
-------------------------------------

..  warning::
    バージョン 0.7.0 は以前のバージョンからいくつかの重要な変更が行われました。
    マイグレーションを検討する際には必ず以下の内容を確認してください。

.. _v07-versioning-sysytem-changing:

Asakusa Frameworkのバージョン体系の変更
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

バージョン 0.7.0 から Hadoop2系への対応が正式対応となりました。
正式にサポートするHadoopのバージョンラインが複数になったことに伴い、Asakusa Frameworkのバージョン体系が以下のようになりました。

Hadoop1系向けAsakusa Framework
  Asakusa Framework バージョン ``0.x.x-hadoop1`` というバージョン名でリリース

Hadoop2系向けAsakusa Framework
  Asakusa Framework バージョン ``0.x.x-hadoop2`` というバージョン名でリリース

バージョン ``0.6.x`` 以前ではHadoop1系向けAsakusa Frameworkは バージョン ``0.x.x`` のようなバージョン体系でリリースされていましたが、バージョン ``0.7.0`` 以降ではHadoop1系向けAsakusa Frameworkは バージョン ``0.x.x-hadoop1`` のように **バージョン番号に '-hadoop1' という接尾辞を指定** するように変更されています。

``0.6.x`` 以前のバージョンからのマイグレーションを行う際には、アプリケーションプロジェクトのビルドスクリプト ( :file:`build.gradle` ) などに含まれるAsakusa Frameworkバージョンに必ず新しいバージョン体系を指定してください。

なおHadoop2系向けのAsakusa Frameworkは以前のバージョンと同様に、バージョン番号に ``-hadoop2`` という接尾辞を指定します。

例として、利用するAsakusa Framework を ``0.6.2`` から ``0.7.0`` に変更する場合の :file:`build.gradle` のバージョン指定を示します。

..  code-block:: groovy
    :caption: build.gradle : Asakusa Framework ``0.6.2`` を利用するプロジェクトのバージョン指定
    :name: build.gradle-previous-migration-guide-1

    asakusafw {
        asakusafwVersion '0.6.2'

..  code-block:: groovy
    :caption: build.gradle : Asakusa Framework ``0.7.0`` を利用するプロジェクトのバージョン指定
    :name: build.gradle-previous-migration-guide-2

    asakusafw {
        asakusafwVersion '0.7.0-hadoop1'

..  attention::
    ``build.gradle`` には先頭に ``buildscript`` ブロックでAsakusa Gradle Plugin のバージョンを指定しますが、このバージョンはAsakusa Frameworkのバージョン指定とは異なり、以下のようにバージョン番号に ``-hadoop1`` や ``-hadoop2`` といった接尾辞は付かないことに注意してください。

..  code-block:: groovy
    :caption: build.gradle
    :name: build.gradle-previous-migration-guide-3

    buildscript {
        repositories {
            maven { url 'http://asakusafw.s3.amazonaws.com/maven/releases' }
        }
        dependencies {
            classpath group: 'com.asakusafw', name: 'asakusa-gradle-plugins', version: '0.7.0'
        }
    }

開発環境と運用環境で異なるHadoopバージョンを使用する
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

バージョン 0.7.0 では、開発環境で使用するHadoopはHadoop1系の利用を推奨しています。

また、:jinrikisha:`Jinrikisha <index.xml>` や :doc:`../introduction/start-guide` の手順に従ってセットアップされる開発環境はHadoop1系が利用されます。

..  note::
    開発環境でHadoop1系の利用を推奨している主な理由は、現時点では開発環境の動作検証で使用しているHadoopプラットフォームにおいて、スタンドアロンモードの実行効率がHadoop1系のほうが良好であると判断しているためです。

バージョン ``0.6.x`` 以前では 開発環境と運用環境で異なるHadoopバージョンを利用する場合に、開発環境を構成するアプリケーションプロジェクトとは別に、運用環境を構成する独立したプロジェクトの作成を推奨していました。

バージョン ``0.7.0`` からはそれぞれの環境の構成を「プロファイル」として記述し、アプリケーションプロジェクト内で複数のプロファイルを管理する方式を推奨しています。

プロファイルの利用方法(例えば開発環境でHadoop1系を利用し、運用環境でHadoop2系を利用するための設定)については、以下のドキュメントを参照してください。

* :doc:`../administration/deployment-guide`

また、この変更に伴いバージョン ``0.6.x`` で提供していたAsakusa Gradle Pluginの一部機能が非推奨になりました。
非推奨となったAsakusa Gradle Pluginについては、以下のドキュメントを参照してください。

* :doc:`gradle-plugin-deprecated`

標準設定で使用するJavaバージョンの変更
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

バージョン 0.7.0 から Asakusa Gradle Pluginでアプリケーションプロジェクトに設定されるJavaバージョンが JDK6 から JDK7 に変更になりました。

Java 7に対応していないHadoopディストリビューション上でアプリケーションを実行する場合、プロジェクトの設定でJDK 6を利用するよう変更する必要があります。

プロジェクトで利用するJavaバージョンをJDK 6に変更する手順は、以下のドキュメントを参照してください。

* :doc:`using-jdk`

プロジェクトテンプレートのGradleバージョンの変更
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

バージョン 0.7.0 から プロジェクトテンプレートに標準で設定されるGradleバージョンが 1.12 から 2.1 に変更になりました。

過去のバージョンで作成したアプリケーションプロジェクトのGradleのバージョンを変更する場合は、以下のドキュメントを参照してください。

* :doc:`gradle-plugin` - :ref:`vup-gradle-wrapper`

..  attention::
    バージョン 0.6.2 以前のAsakusa FrameworkはGradle 2.0以降に対応していません。
    プロジェクトのAsakusa Frameworkのバージョンをダウングレードする場合は、必ず利用するGradleのバージョンを合わせて変更してください

Mavenプロジェクトの非推奨化
~~~~~~~~~~~~~~~~~~~~~~~~~~~

バージョン 0.7.0 から Mavenプロジェクトは非推奨になりました。

本バージョンにおいては、 MavenプロジェクトからでもAsakusa Frameworkの多くの機能は従来通り利用できますが、本バージョンからMavenプロジェクト特有の機能の動作検証は行われなくなったため、Gradleプロジェクトへの移行を強く推奨します。

MavenプロジェクトからGradleプロジェクトへのマイグレーション手順については、 :doc:`gradle-plugin` - :ref:`migrate-from-maven-to-gradle` を参照してください。

..  attention::
    バージョン 0.7.0 のドキュメントから動作検証が行われていない一部のMavenプロジェクト向け機能に関する説明が削除されました。

0.6.2 へのマイグレーション
--------------------------
バージョン0.6.2におけるバージョン固有のマイグレーション手順はありません。
各バージョン共通のマイグレーション手順のみを実施してください。

0.6.1 へのマイグレーション
--------------------------
バージョン0.6.1におけるバージョン固有のマイグレーション手順はありません。
各バージョン共通のマイグレーション手順のみを実施してください。

0.6.0 へのマイグレーション
--------------------------
バージョン0.6.0ではプロジェクトテンプレートに含まれるファイルに対して
細かな修正やディレクトリ構成の変更が行われたため、
バージョン0.6.0で提供しているプロジェクトテンプレートの内容に置き換えることを推奨します。

プロジェクトテンプレートの置き換えについては、
:doc:`gradle-plugin` - :ref:`apply-gradle-project-template` の項などを参照してください。

0.5.3 へのマイグレーション
--------------------------

バージョン 0.5.3 以前については、過去バージョンのドキュメントを参照してください。
