# parent-properties

## 概要
`<parent>` に変数を入れた場合の実験

```
<parent>
  <groupId>com.fasterxml.jackson</groupId>
  <artifactId>jackson-base</artifactId>
  <version>${revision}</version>
</parent>
```

このように `${revision}` を入れている。
`${revision}` は `<properties>` で正しく定義されている。

```bigquery
    <properties>
        <revision>2.12.0</revision>
    </properties>
```

`com.fasterxml.jackson:jackson-base:2.12.0` は正しく存在するartifact

https://repo1.maven.org/maven2/com/fasterxml/jackson/jackson-base/2.12.0/jackson-base-2.12.0.pom

## 結果

```
root@326e45dd3791:/app/parent-properties# mvn install
[INFO] Scanning for projects...
Downloading from central: https://repo.maven.apache.org/maven2/com/fasterxml/jackson/jackson-base/$%7Brevision%7D/jackson-base-$%7Brevision%7D.pom
[ERROR] [ERROR] Some problems were encountered while processing the POMs:
[FATAL] Non-resolvable parent POM for com.example:project:1.0-SNAPSHOT: Could not find artifact com.fasterxml.jackson:jackson-base:pom:${revision} in central (https://repo.maven.apache.org/maven2) and 'parent.relativePath' points at wrong local POM @ line 11, column 13
 @
[ERROR] The build could not read 1 project -> [Help 1]
[ERROR]
[ERROR]   The project com.example:project:1.0-SNAPSHOT (/app/parent-properties/pom.xml) has 1 error
[ERROR]     Non-resolvable parent POM for com.example:project:1.0-SNAPSHOT: Could not find artifact com.fasterxml.jackson:jackson-base:pom:${revision} in central (https://repo.maven.apache.org/maven2) and 'parent.relativePath' points at wrong local POM @ line 11, column 13 -> [Help 2]
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/ProjectBuildingException
[ERROR] [Help 2] http://cwiki.apache.org/confluence/display/MAVEN/UnresolvableModelException
```

`${revision}` という文字列のままjackson-baseの検索に行っていることがログから明らかなので、 `<parent>` 内の変数は展開されないと思われる。