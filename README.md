# LearnWorldSampleDatabase--從問題解決中學**資料庫**
## 問題集

### 起手式
1. XAMPP環境安裝好了嗎？
2. MySQL環境安裝好了嗎？
3. MySQL要如何啟動？
4. 如何進入MySQL的CLI？

### 匯入 MySQL Sample 資料庫
1. 如何下載 world.sql？
2. 如何在 CLI 執行 world.sql？
3. world.sql 執行的結果是什麼？
4. 進入 CLI 後如何切換到 world 資料庫？

### 瞭解 WORLD 資料庫的 綱要 SCHEMA
1. world資料庫共有幾個表格TABLES？
2. world資料庫有那些表格TABLES？
3. 什麼是主鍵(PK, primary key)？
4. country表格有那些欄位 Column/Field？
5. 承上，PK為何？
6. 承上，有那些欄位型態？
7. city表格有那些欄位？
8. 承上，PK為何？
9. 承上，有那些欄位型態？
10. countrylanguage表格有那些欄位？
11. 承上，PK為何？
12. 承上，有那些欄位型態？
13. 什麼是外來鍵(FK, forieng key)？
14. world資料庫的表格裡，那些是FK？
15. FK還可以看那些細節？

### 瞭解 WORLD 資料庫的 記錄 Record/Row
1. 共有多少個國家？
2. 共有多少個城市？
3. 共有多少個語言？

- - -

## SQL指令練習
### world.sql內容

### country 國家表格
1. 如何請列出所有國家？欄位如下：
```
  +------+----------------------------------------------+---------------+-----------+------------+
  | code | name                                         | continent     | indepyear | population |
  +------+----------------------------------------------+---------------+-----------+------------+
  | ABW  | Aruba                                        | North America |      NULL |     103000 |
  | AFG  | Afghanistan                                  | Asia          |      1919 |   22720000 |
            ...中間略...
  | ZMB  | Zambia                                       | Africa        |      1964 |    9169000 |
  | ZWE  | Zimbabwe                                     | Africa        |      1980 |   11669000 |
  +------+----------------------------------------------+---------------+-----------+------------+
``` 

2. 如何列出人口大於5千萬人的國家？欄位如下：
```
  +---------------------------------------+------------+
  | name                                  | population |
  +---------------------------------------+------------+
  | Ukraine                               |   50456000 |
  | Congo, The Democratic Republic of the |   51654000 |
              ...中間略...
  | India                                 | 1013662000 |
  | China                                 | 1277558000 |
  +---------------------------------------+------------+
```

3. 如何顯示出各個洲各有多少個國家？欄位如下：
```
  +---------------+--------------+
  | continent     | CountryCount |
  +---------------+--------------+
  | Asia          |           51 |
  | Europe        |           46 |
  | North America |           37 |
  | Africa        |           58 |
  | Oceania       |           28 |
  | Antarctica    |            5 |
  | South America |           14 |
  +---------------+--------------+
```

4. 如何顯示國家的**人口/國土比例**，並由小排到大？
```
+----------------------------------------------+-------------------+
| name                                         | people_land_ratio |
+----------------------------------------------+-------------------+
| French Southern territories                  |            0.0000 |
| United States Minor Outlying Islands         |            0.0000 |
             ...中間略...
| Mongolia                                     |            1.6993 |
| French Guiana                                |            2.0111 |
             ...中間略...

| Hong Kong                                    |         6308.8372 |
| Monaco                                       |        22666.6667 |
| Macao                                        |        26277.7778 |
+----------------------------------------------+-------------------+
```
- - -
## WORLD資料庫SCHEMA
```
MariaDB [world]> show tables;
+-----------------+
| Tables_in_world |
+-----------------+
| city            |城市
| country         |國家
| countrylanguage |國語
+-----------------+
3 rows in set (0.001 sec)
MariaDB [world]> describe city;
+-------------+----------+------+-----+---------+----------------+
| Field       | Type     | Null | Key | Default | Extra          |
+-------------+----------+------+-----+---------+----------------+
| ID          | int(11)  | NO   | PRI | NULL    | auto_increment |識別碼
| Name        | char(35) | NO   |     |         |                |城市名
| CountryCode | char(3)  | NO   | MUL |         |                |國家碼
| District    | char(20) | NO   |     |         |                |地域
| Population  | int(11)  | NO   |     | 0       |                |人口數
+-------------+----------+------+-----+---------+----------------+
5 rows in set (0.015 sec)

MariaDB [world]> describe country;
+----------------+---------------------------------------------------------------------------------------+------+-----+---------+-------+
| Field          | Type                                                                                  | Null | Key | Default | Extra |
+----------------+---------------------------------------------------------------------------------------+------+-----+---------+-------+
| Code           | char(3)                                                                               | NO   | PRI |         |       |國家碼
| Name           | char(52)                                                                              | NO   |     |         |       |國家名
| Continent      | enum('Asia','Europe','North America','Africa','Oceania','Antarctica','South America') | NO   |     | Asia    |       |洲際
| Region         | char(26)                                                                              | NO   |     |         |       |區域
| SurfaceArea    | decimal(10,2)                                                                         | NO   |     | 0.00    |       |表面積
| IndepYear      | smallint(6)                                                                           | YES  |     | NULL    |       |獨立年份
| Population     | int(11)                                                                               | NO   |     | 0       |       |人口數
| LifeExpectancy | decimal(3,1)                                                                          | YES  |     | NULL    |       |壽命
| GNP            | decimal(10,2)                                                                         | YES  |     | NULL    |       |國民所得
| GNPOld         | decimal(10,2)                                                                         | YES  |     | NULL    |       |國民所得舊
| LocalName      | char(45)                                                                              | NO   |     |         |       |當地名稱
| GovernmentForm | char(45)                                                                              | NO   |     |         |       |政府形式
| HeadOfState    | char(60)                                                                              | YES  |     | NULL    |       |
| Capital        | int(11)                                                                               | YES  |     | NULL    |       |首都
| Code2          | char(2)                                                                               | NO   |     |         |       |國家碼2
+----------------+---------------------------------------------------------------------------------------+------+-----+---------+-------+
15 rows in set (0.013 sec)

MariaDB [world]> describe countrylanguage;
+-------------+---------------+------+-----+---------+-------+
| Field       | Type          | Null | Key | Default | Extra |
+-------------+---------------+------+-----+---------+-------+
| CountryCode | char(3)       | NO   | PRI |         |       |
| Language    | char(30)      | NO   | PRI |         |       |
| IsOfficial  | enum('T','F') | NO   |     | F       |       |
| Percentage  | decimal(4,1)  | NO   |     | 0.0     |       |
+-------------+---------------+------+-----+---------+-------+
4 rows in set (0.014 sec)

MariaDB [world]>
```
