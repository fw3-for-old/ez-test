# Flywheel3 Eazy Unit test tool for old php versions

Rapid Development FrameworkであるFlywheel3 の簡易Unit test ツールです。

## 対象バージョンおよび動作確認バージョン

対象バージョン：PHP5.3.3以降

### 動作確認バージョン


- **5.3.3**
- 5.3.4
- 5.3.5
- 5.3.6
- 5.3.7
- 5.3.8
- **5.3.9**
- 5.4.16
- 5.4.39
- **5.4.45**
- **5.5.38**
- **5.6.40**
- **7.0.33**
- **7.1.33**
- **7.2.33**
- **7.3.21**
- **7.4.0**
- **7.4.9**
- **8.0.0**
- **8.0.3**

## 主な機能

### TestRunner

Windows、Unix系OSを問わずに動作するPHPUnitに似たアサーションを持つUnitTest実行器です。

テストケース内のNGとなった行と行が含まれるメソッド名までを出力するため、NG発生時の追跡が用意になっています。

また、PHP5.3.0以降、少なくともPHP8.0.3までは確実に動作するため、PHPのVersion Upに伴う、事前・事後検証を簡単に行う事が出来ます。

### 実行後例（成功時）

```
================================================
 fw3_for_old/ez_test.
 target test cases => C:\Users\xxx\fw3-for-old\strings\tests
================================================
 start time  : 2021/04/01 00:07:34.7802
 end time    : 2021/04/01 00:07:34.8099
 exec time   : 0.029723882675171sec
================================================

================================================
 test result: success (411 / 411)
------------------------------------------------
 details
------------------------------------------------
  test class:fw3_for_old\tests\strings\builder\modifys\datetime\DateModifierTest (6 / 6)
  test class:fw3_for_old\tests\strings\builder\modifys\datetime\StrtotimeModifierTest (6 / 6)
  test class:fw3_for_old\tests\strings\builder\modifys\security\EscapeModifierTest (6 / 6)
  test class:fw3_for_old\tests\strings\cases\builder\StringBuilderTest (270 / 270)
  test class:fw3_for_old\tests\strings\converter\ConvertTest (123 / 123)
================================================

================================================
 test has been finished.
================================================
```

### 実行後例（失敗時）

```
================================================
 fw3_for_old/ez_test.
 target test cases => C:\Users\xxx\fw3-for-old\strings\tests
================================================
 start time  : 2021/04/01 00:31:52.6694
 end time    : 2021/04/01 00:31:52.6996
 exec time   : 0.030210971832275sec
================================================

================================================
 test result: failed (410 / 411)
------------------------------------------------
 details
------------------------------------------------
  test class:fw3_for_old\tests\strings\builder\modifys\datetime\DateModifierTest (6 / 6)
  test class:fw3_for_old\tests\strings\builder\modifys\datetime\StrtotimeModifierTest (6 / 6)
  test class:fw3_for_old\tests\strings\builder\modifys\security\EscapeModifierTest (5 / 6)
    fw3_for_old\tests\strings\builder\modifys\security\EscapeModifierTest->testModify() in line 39
      expected: '<a href=&quot;#id&quot; onclick=&quot;alert(&#039;alert&#039;);&quot;&gt;'
      actual:   '&lt;a href=&quot;#id&quot; onclick=&quot;alert(&#039;alert&#039;);&quot;&gt;'
  test class:fw3_for_old\tests\strings\cases\builder\StringBuilderTest (270 / 270)
  test class:fw3_for_old\tests\strings\converter\ConvertTest (123 / 123)
================================================

================================================
 test has been finished.
================================================
```

## 使い方

### 1 . インストール

#### composerを使用できる環境の場合

次のコマンドを実行し、インストールしてください。

`composer require fw3_for_old/ez_test`

#### composerを使用できない環境の場合

[Download ZIP](https://github.com/fw3-for-old/ez_test/archive/master.zip)よりzipファイルをダウンロードし、任意のディレクトリにコピーしてください。

使用対象となる処理より前に`require_once sprintf('%s/src/ez_test_require_once.php', $path_to_copy_dir);`として`src/ez_test_require_once.php`を読み込むようにしてください。

### 2 . テストの作成と実施

1. 次の構造のディレクトリツリーとなっている前提で解説します。

この構造のうち、`composer.json`が置いてあるディレクトリをカレントディレクトリとします。

実用にあたっては適宜読み替えてください。

```
composer.json
vendor/fw3_for_old/ez_test/src/ez_test_require_once.php
vendor/fw3_for_old/ez_test/src/TestRunner.php
vendor/fw3_for_old/ez_test/src/test_unit
vendor/fw3_for_old/ez_test/src/test_unit/AbstractTest.php
vendor/fw3_for_old/ez_test/src/test_unit/TestInterface.php
```

併せて次のディレクトリを作成してください。

```
tests/cases
```


2. `tests`ディレクトリにブートストラップファイルを作成します。

次の内容で`run_test.php`としてファイルを作成してください。

composerを使用できる環境の場合
```
<?php

namespace fw3_for_old\ez_test;

require_once sprintf('%s/vendor/autoload.php', dirname(__DIR__));

TestRunner::factory()->run();
```

composerを使用できない環境の場合
```
<?php

namespace fw3_for_old\ez_test;

require_once sprintf('%s/vendor/fw3_for_old/ez_test/src/ez_test_require_once.php', dirname(__DIR__));

TestRunner::factory()->run();
```

3. `tests/cases`ディレクトリにテストケースを作成します。

`tests/cases`ディレクトリの中に`\fw3_for_old\test_unit\AbstractTest`を継承したクラスを作成してください。
その際、テストケースとテストクラス名は同一にしてください。

テストクラスは末尾が`Test`である必要があります。

併せてテストクラスの中にテストメソッドを記述してください。

テストメソッドは`test`から始まるメソッド名である必要があります。

例
```
<?php

class EzTestToolTest extends \fw3_for_old\ez_test\test_unit\AbstractTest
{
    /**
     * このクラスでテストを始める前に呼ばれるメソッド
     */
    public function setUp()
    {
    }

    /**
     * このクラスでテストが終わった後に呼ばれるメソッド
     */
    public function tearDown()
    {
    }

    /**
     * 各テストメソッドが実行される前に呼ばれるメソッド
     */
    public function init()
    {
    }

    /**
     * 各テストメソッドが実行された後に呼ばれるメソッド
     */
    public function cleanUp()
    {
    }

    /**
     * テストメソッド
     *
     * `test`から始まるメソッドはテストメソッドとして自動実行されます
     */
    public function testBuild()
    {
        $this->assertSame(1, 1); // assertから始まるメソッドがアサーション用メソッドです
    }
}
```

`tests/cases`以下にディレクトリはあってもなくても構いません。

`fw3_for_old/ez_test`はデフォルトでは`TestRunner::factory()->run();`があるファイルと同じディレクトリの中からテスト対象を発見しようとします。

対象となるディレクトリは次の優先度で決定します。

- `cases`ディレクトリ
- `case`ディレクトリ
- `TestRunner::factory()->run();`があるディレクトリ

対象となるディレクトリを決定後、ファイル名およびクラス名の末尾が`Test`でかつ`\fw3_for_old\ez_test\test_unit\TestInterface`を実装したクラスを検出し、テスト対象として認識します。

4. テストの実施

PHP CLIでブートストラップファイルを実行します。

例

```
php tests/run_test.php
```

これでテストの作成と実施は完了です。

### 3. 特殊な使い方

#### 複数のディレクトリにあるテストを逐次実行させたい。

`testCaseRootDir`メソッドを用いて、テストケースがあるディレクトリパスを指定してください。

```
<?php
TestRunner::factory()->testCaseRootDir($path_to_test_cases_dir_1)->run();
TestRunner::factory()->testCaseRootDir($path_to_test_cases_dir_2)->run();
```
