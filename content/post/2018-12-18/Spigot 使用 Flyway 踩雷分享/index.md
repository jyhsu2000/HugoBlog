---
title: Spigot 使用 Flyway 踩雷分享
date: 2018-12-18
categories: Spigot
tags:
    - Java
    - Minecraft
    - Spigot
---
[Flyway](https://flywaydb.org/) 是一款資料庫版本控管的解決方案，  
方便接上不同類型的資料庫，也能夠輕易實作遷移機制（Migration）

自己撰寫的 Spigot 插件正好需要連結資料庫  
原本苦惱於遷移機制的實作，直到發現 Flyway 具備遷移機制，同時又具有 Java API 可以使用  
就決定是這款了！

這篇主要是要記錄自己踩雷的解決方法，就不推坑傳教了  
有興趣自己去 Google

---

在這次撰寫的 Spigot 插件中，打算暫且先以 SQLite 作為資料庫  
於是先安裝了 Flyway 與 SQLite-JDBC

```
compile 'org.flywaydb:flyway-core:5.2.4'
compile 'org.xerial:sqlite-jdbc:3.23.1'
```

設定 Flyway

```java
String configFolder = this.plugin.getDataFolder().getAbsolutePath();
String dbUrl = "jdbc:sqlite:" + configFolder + "/database.db";

Flyway flyway = Flyway.configure().dataSource(dbUrl, null, null).load();
flyway.migrate();
```

然後他就爆炸了ლ(ﾟдﾟლ)

---

遇到的第一個問題：

```
FlywayException: Unable to instantiate class org.flywaydb.core.internal.logging.slf4j.Slf4jLogCreator
```

解決方案：自行指定 LogCreator  
[https://github.com/flyway/flyway/issues/506](https://github.com/flyway/flyway/issues/506)

```java
LogFactory.setLogCreator(new Slf4jLogCreator());
```

遇到的第二個問題：

```
Unable to resolve location classpath:db/migration
```

解決方案：自行指定 ClassLoader

```java
Flyway flyway = Flyway.configure(getClass().getClassLoader()).dataSource(dbUrl, null, null).load();
```

---

就這樣，花了半天解決這些問題  
還不錯用，就是資料難找了點…
