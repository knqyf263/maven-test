# module-parent-parent

## 概要
multi-moduleにおいてaggregationがさらにparentを持っている場合

```
parent
    \--> aggregation
                  \--> module
```

[module-parent-parent](../module-parent-parent) との差分はparentの `<version>` を変数使わずにベタ書きしたこと。

```
<version>1.0-SNAPSHOT</version>
```

元は `${revision}` を使っていた。この `${revision}` は1.0-SNAPSHOTなので等しいはず。
差分はその点のみ。

## 結果

```
root@326e45dd3791:/app/module-parent-parent# mvn dependency:tree
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
[INFO] Aggregation ........................................ SUCCESS [  0.814 s]
[INFO] Module ............................................. SUCCESS [  0.030 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.220 s
[INFO] Finished at: 2021-02-05T07:28:54Z
[INFO] ------------------------------------------------------------------------
```

成功する。aggregationはparentから `<version>` を正しく継承しているが、その中身が `${revision}` なのか `1.0-SNAPSHOT` なのか検証が必要。