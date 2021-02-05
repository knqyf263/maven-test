# module-parent-properties2

## 概要
multi-moduleにおいて `<parent>` に変数を入れ、aggregation側の `<version>` はベタ書きする場合の実験

module側には以下のように定義する。
```
<parent>
  <groupId>com.example</groupId>
  <artifactId>aggregation</artifactId>
  <version>${revision}</version>
</parent>
```

aggregation側は以下のようにする。

```
    <groupId>com.example</groupId>
    <artifactId>aggregation</artifactId>
    <version>1.0-SNAPSHOT</version>
```

元は

```
    <groupId>com.example</groupId>
    <artifactId>aggregation</artifactId>
    <version>${revision}</version>
```

だったが、`${revision}` は以下のように定義されているため、上の2つは同値になるはず。
[module-parent-properties](../module-parent-properties) との差分はここのみ。

```bigquery
    <properties>
        <revision>1.0-SNAPSHOT</revision>
    </properties>
```


## 結果

```
root@326e45dd3791:/app/module-parent-properties2# mvn dependency:tree
[INFO] Scanning for projects...
Downloading from central: https://repo.maven.apache.org/maven2/com/example/aggregation/$%7Brevision%7D/aggregation-$%7Brevision%7D.pom
[ERROR] [ERROR] Some problems were encountered while processing the POMs:
[FATAL] Non-resolvable parent POM for com.example:module:1.0-SNAPSHOT: Could not find artifact com.example:aggregation:pom:${revision} in central (https://repo.maven.apache.org/maven2) and 'parent.relativePath' points at wrong local POM @ line 11, column 13
 @
[ERROR] The build could not read 1 project -> [Help 1]
[ERROR]
[ERROR]   The project com.example:module:1.0-SNAPSHOT (/app/module-parent-properties2/module/pom.xml) has 1 error
[ERROR]     Non-resolvable parent POM for com.example:module:1.0-SNAPSHOT: Could not find artifact com.example:aggregation:pom:${revision} in central (https://repo.maven.apache.org/maven2) and 'parent.relativePath' points at wrong local POM @ line 11, column 13 -> [Help 2]
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/ProjectBuildingException
[ERROR] [Help 2] http://cwiki.apache.org/confluence/display/MAVEN/UnresolvableModelException
```

失敗する。エラーメッセージを見ると、 `aggregation-\$%7Brevision%7D.pom` を探しに行っていることが分かる。
このことから、moduleの `<parent>` 内の `${revision}` は展開できた結果、parentの検索に成功したのではなく `${revision}` という文字列のまま検索しているのではないかという仮説が立てられる。
これは [parent-properties](../parent-properties) の `<parent>` 内の変数は展開されないという結果とも矛盾しない。
また、 [module-inheritance](../module-inheritance) のaggregationの `<properties>` は継承されないという結果とも矛盾しない。
