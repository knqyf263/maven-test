# module-parent-properties3

## 概要
multi-moduleにおいて `<version>` にpropertiesを入れ、module側の<parent>はベタ書きする場合の実験

module側には `<parent>` を以下のように定義する。
```
<parent>
  <groupId>com.example</groupId>
  <artifactId>aggregation</artifactId>
  <version>1.0-SNAPSHOT</version>
</parent>
```

[module-parent-properties](../module-parent-properties) では以下のように変数を使っていた。

```
<parent>
  <groupId>com.example</groupId>
  <artifactId>aggregation</artifactId>
  <version>${revision}</version>
</parent>
```

`${revision}` は以下のように定義されているため、上の2つは同義になるはず。
[module-parent-properties](../module-parent-properties) との差分はここのみ。

```bigquery
    <properties>
        <revision>1.0-SNAPSHOT</revision>
    </properties>
```


## 結果

```
root@326e45dd3791:/app/module-parent-properties3# mvn dependency:tree
[INFO] Scanning for projects...
[ERROR] [ERROR] Some problems were encountered while processing the POMs:
[FATAL] Non-resolvable parent POM for com.example:module:1.0-SNAPSHOT: Could not find artifact com.example:aggregation:pom:1.0-SNAPSHOT and 'parent.relativePath' points at wrong local POM @ line 11, column 13
 @
[ERROR] The build could not read 1 project -> [Help 1]
[ERROR]
[ERROR]   The project com.example:module:1.0-SNAPSHOT (/app/module-parent-properties3/module/pom.xml) has 1 error
[ERROR]     Non-resolvable parent POM for com.example:module:1.0-SNAPSHOT: Could not find artifact com.example:aggregation:pom:1.0-SNAPSHOT and 'parent.relativePath' points at wrong local POM @ line 11, column 13 -> [Help 2]
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/ProjectBuildingException
[ERROR] [Help 2] http://cwiki.apache.org/confluence/display/MAVEN/UnresolvableModelException
```

失敗する。 エラーメッセージを見ると、 `com.example:aggregation:pom:1.0-SNAPSHOT` を正しく検索に行っていることが分かる。
しかし失敗したことを考えると、parentの `${revision}` はまだこの段階では展開されておらず `${revision}` という文字列と `1.0-SNAPSHOT` という文字列の比較によりマッチしなかったのではないかと予想できる。
このことから、moduleの `<parent>` 内の `${revision}` は展開できた結果、parentの検索に成功したのではなく `${revision}` という文字列のまま検索しているのではないかという仮説が立てられる。
これは [parent-module-properties2](../parent-module-properties2) の仮説と矛盾しない。
