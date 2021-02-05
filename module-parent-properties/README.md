# module-parent-properties

## 概要
multi-moduleにおいて `<parent>` に変数を入れた場合の実験

```
<parent>
  <groupId>com.example</groupId>
  <artifactId>aggregation</artifactId>
  <version>${revision}</version>
</parent>
```

このように `${revision}` を入れている。
`${revision}` はmoduleでは定義されておらず、aggregationの `<properties>` に定義されている。

```bigquery
    <properties>
        <revision>1.0-SNAPSHOT</revision>
    </properties>
```


## 結果

```
root@326e45dd3791:/app/module-parent-properties# mvn dependency:tree
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] Aggregation                                                        [pom]
[INFO] Module                                                             [pom]
[INFO]
[INFO] ----------------------< com.example:aggregation >-----------------------
[INFO] Building Aggregation 1.0-SNAPSHOT                                  [1/2]
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ aggregation ---
[INFO] com.example:aggregation:pom:1.0-SNAPSHOT
[INFO]
[INFO] -------------------------< com.example:module >-------------------------
[INFO] Building Module 1.0-SNAPSHOT                                       [2/2]
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ module ---
[INFO] com.example:module:pom:1.0-SNAPSHOT
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary for Aggregation 1.0-SNAPSHOT:
[INFO]
[INFO] Aggregation ........................................ SUCCESS [  0.660 s]
[INFO] Module ............................................. SUCCESS [  0.039 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.113 s
[INFO] Finished at: 2021-02-05T06:38:16Z
[INFO] ------------------------------------------------------------------------
```

成功する。parentが正しく検索できていることから、 `${revision}` が展開できているような挙動に見える。
しかし [parent-properties](../parent-properties) の結果から `<parent>` 内の変数は展開されないことが分かっているため、この結果は矛盾する。
また、 [module-inheritance](../module-inheritance) の結果からaggregationの `<properties>` はmodule側には伝搬しないことも分かっているため、仮に `<parent>` 内の変数が展開されるとしてもやはりその結果とも矛盾する（revisionはmodule内で定義されていない変数のため）。
