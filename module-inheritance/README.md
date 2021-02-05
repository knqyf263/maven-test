# module-inheritance

## 概要
multi-moduleにおいてaggregationから `<properties>` が引き継がれるかの検証

```
<dependencies>
    <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>${jackson.version}</version>
    </dependency>
</dependencies>
```

このように `${jackson.version}` を入れている。
`${jackson.version}` はmoduleには定義されておらず、aggregationの `<properties>` で定義されている。

```bigquery
    <properties>
        <jackson.version>2.12.0</jackson.version>
    </properties>
```

`com.fasterxml.jackson.core:jackson-databind:2.12.0` は正しく存在するPOM  
https://repo1.maven.org/maven2/com/fasterxml/jackson/core/jackson-databind/2.12.0/jackson-databind-2.12.0.pom

## 結果

```
root@326e45dd3791:/app/module-inheritance# mvn dependency:tree
[INFO] Scanning for projects...
[ERROR] [ERROR] Some problems were encountered while processing the POMs:
[ERROR] 'dependencies.dependency.version' for com.fasterxml.jackson.core:jackson-databind:jar must be a valid version but is '${jackson.version}'. @ line 16, column 22
 @
[ERROR] The build could not read 1 project -> [Help 1]
[ERROR]
[ERROR]   The project com.example:module:1.0-SNAPSHOT (/app/module-inheritance/module/pom.xml) has 1 error
[ERROR]     'dependencies.dependency.version' for com.fasterxml.jackson.core:jackson-databind:jar must be a valid version but is '${jackson.version}'. @ line 16, column 22
[ERROR]
[ERROR] To see the full stack trace of the errors, re-run Maven with the -e switch.
[ERROR] Re-run Maven using the -X switch to enable full debug logging.
[ERROR]
[ERROR] For more information about the errors and possible solutions, please read the following articles:
[ERROR] [Help 1] http://cwiki.apache.org/confluence/display/MAVEN/ProjectBuildingException
```

`${jackson.version}` が展開されておらず失敗する。
`<dependencies>` をaggregation側に書くと成功するため `<properties>` は正しく定義できている。
このことからmulti-module構成でaggregationの `<properties>` は継承されないことが分かる。
同様に `<dependencies>` など他の要素も継承されない。
