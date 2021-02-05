# module-parent-parent2

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
root@326e45dd3791:/app/module-parent-parent2# mvn dependency:tree
[INFO] Scanning for projects...
[ERROR] [ERROR] Some problems were encountered while processing the POMs:
[FATAL] Non-resolvable parent POM for com.example:aggregation:${revision}: Failure to find com.example:parent:pom:${revision} in https://repo.maven.apache.org/maven2 was cached in the local repository, resolution will not be reattempted until the update interval of central has elapsed or updates are forced and 'parent.relativePath' points at wrong local POM @ line 12, column 13
 @
[ERROR] The build could not read 1 project -> [Help 1]
[ERROR]
[ERROR]   The project com.example:aggregation:${revision} (/app/module-parent-parent2/pom.xml) has 1 error
[ERROR]     Non-resolvable parent POM for com.example:aggregation:${revision}: Failure to find com.example:parent:pom:${revision} in https://repo.maven.apache.org/maven2 was cached in the local repository, resolution will not be reattempted until the update interval of central has elapsed or updates are forced and 'parent.relativePath' points at wrong local POM @ line 12, column 13 -> [Help 2]
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/ProjectBuildingException
[ERROR] [Help 2] http://cwiki.apache.org/confluence/display/MAVEN/UnresolvableModelException
```

失敗する。 `Failure to find com.example:parent:pom:${revision}` のエラーから `${revision}` で探しに行って見つからないことが分かる。
aggregationはparentから `<version>` を正しく継承しているが、その中身は正確に `${revision}` である必要がある。
module側は `${revision}` という文字列で検索に行くため、例え変数展開後は同値でも `1.0-SNAPSHOT` にしてしまうと文字列比較が失敗してしまう。
