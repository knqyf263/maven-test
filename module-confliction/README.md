# module-confliction

## 概要
multi-moduleにおいてmodule同士でdependencyが衝突している場合の実験

まずmoduleを2つ指定。

```
    <modules>
        <module>module1</module>
        <module>module2</module>
    </modules>

```

それぞれのmoduleで異なるバージョンを指定。

```
    <dependencies>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.12.0</version>
        </dependency>
    </dependencies>
```

```
    <dependencies>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.10.0</version>
        </dependency>
    </dependencies>
```


## 結果

```
[INFO] Scanning for projects...
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Build Order:
[INFO]
[INFO] Module 1                                                           [pom]
[INFO] Module 2                                                           [pom]
[INFO] Aggregation                                                        [pom]
[INFO]
[INFO] ------------------------< com.example:module1 >-------------------------
[INFO] Building Module 1 1.0-SNAPSHOT                                     [1/3]
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ module1 ---
[INFO] com.example:module1:pom:1.0-SNAPSHOT
[INFO] \- com.fasterxml.jackson.core:jackson-databind:jar:2.12.0:compile
[INFO]    +- com.fasterxml.jackson.core:jackson-annotations:jar:2.12.0:compile
[INFO]    \- com.fasterxml.jackson.core:jackson-core:jar:2.12.0:compile
[INFO]
[INFO] ------------------------< com.example:module2 >-------------------------
[INFO] Building Module 2 1.0-SNAPSHOT                                     [2/3]
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ module2 ---
[INFO] com.example:module2:pom:1.0-SNAPSHOT
[INFO] \- com.fasterxml.jackson.core:jackson-databind:jar:2.10.0:compile
[INFO]    +- com.fasterxml.jackson.core:jackson-annotations:jar:2.10.0:compile
[INFO]    \- com.fasterxml.jackson.core:jackson-core:jar:2.10.0:compile
[INFO]
[INFO] ----------------------< com.example:aggregation >-----------------------
[INFO] Building Aggregation 1.0.0                                         [3/3]
[INFO] --------------------------------[ pom ]---------------------------------
[INFO]
[INFO] --- maven-dependency-plugin:2.8:tree (default-cli) @ aggregation ---
[INFO] com.example:aggregation:pom:1.0.0
[INFO] ------------------------------------------------------------------------
[INFO] Reactor Summary:
[INFO]
[INFO] Module 1 1.0-SNAPSHOT .............................. SUCCESS [  1.074 s]
[INFO] Module 2 1.0-SNAPSHOT .............................. SUCCESS [  0.067 s]
[INFO] Aggregation 1.0.0 .................................. SUCCESS [  0.015 s]
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  1.812 s
[INFO] Finished at: 2021-02-08T04:52:01Z
[INFO] ------------------------------------------------------------------------

```

両方のバージョンが独立してインストールされている。つまりmodule同士ではdependencyの解決は行われず、バージョンが異なっていてもそれぞれインストールされる。
