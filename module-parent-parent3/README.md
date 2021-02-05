# module-parent-parent3

## 概要
multi-moduleにおいてaggregationがさらにparentを持っている場合

```
parent
    \--> aggregation
                  \--> module
```

[module-parent-parent](../module-parent-parent) との差分はmoduleの `<version>` に変数使わずにベタ書きしたこと。

```
    <parent>
        <groupId>com.example</groupId>
        <artifactId>aggregation</artifactId>
        <version>1.0-SNAPSHOT</version>
    </parent>
```

元は `${revision}` を使っていた。この `${revision}` は1.0-SNAPSHOTなので等しいはず。
差分はその点のみ。

## 結果

```
root@326e45dd3791:/app/module-parent-parent3# mvn dependency:tree
[INFO] Scanning for projects...
[ERROR] [ERROR] Some problems were encountered while processing the POMs:
[FATAL] Non-resolvable parent POM for com.example:module:1.0-SNAPSHOT: Could not find artifact com.example:aggregation:pom:1.0-SNAPSHOT and 'parent.relativePath' points at wrong local POM @ line 11, column 13
 @
[ERROR] The build could not read 1 project -> [Help 1]
[ERROR]
[ERROR]   The project com.example:module:1.0-SNAPSHOT (/app/module-parent-parent3/module/pom.xml) has 1 error
[ERROR]     Non-resolvable parent POM for com.example:module:1.0-SNAPSHOT: Could not find artifact com.example:aggregation:pom:1.0-SNAPSHOT and 'parent.relativePath' points at wrong local POM @ line 11, column 13 -> [Help 2]
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/ProjectBuildingException
[ERROR] [Help 2] http://cwiki.apache.org/confluence/display/MAVEN/UnresolvableModelException
```

`Could not find artifact com.example:aggregation:pom:1.0-SNAPSHOT` というエラーメッセージから、正しく `1.0-SNAPSHOT` で検索しに行っても失敗していることが分かる。
aggregationはparentから `<version>` を正しく継承しているが、その中身は `${revision}` でありparentの検索のタイミングでは変数展開されていない。
module側は `1.0-SNAPSHOT` という文字列で検索に行くため、例え展開後は同値でも `${revision}` と等しくならず文字列比較が失敗してしまう。
