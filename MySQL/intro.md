# MySQL

- [MySQL](#mysql)
  - [概要](#概要)
  - [コマンドライン](#コマンドライン) - [ログイン](#ログイン) - [データベースの一覧](#データベースの一覧) - [データベースの選択](#データベースの選択) - [選択中のデータベースのテーブル一覧](#選択中のデータベースのテーブル一覧) - [カラム一覧](#カラム一覧) - [例](#例)

---

## 概要

![イルカさん](logo.png)

[MySQL](https://www.mysql.com/jp/)とは, リレーショナルデータベース管理システム(RDBMS)の一つである. 有名な RDBMS としては他にも

- PostgreSQL
- MariaDB
- Microsoft SQL Server
- Oracle Database

などがある.

## コマンドライン

インストールが完了しており, `mysql`に PATH が通っていることを前提とする.

###### ログイン

```
mysql -u (username) -p
```

- `(username)` にユーザ名を入れる. ルートなら`root`
- パスワードを聞かれるので設定時のものを入力
- 完了すると MySQL のコマンドラインインターフェースに入る
- `mysql>`となれば成功

###### データベースの一覧

```
show databases;
```

###### データベースの選択

```
use (databasename);
```

選択中のデータベースの確認は

```
select database();
```

で可能.

###### 選択中のデータベースのテーブル一覧

```
show tables;
```

###### カラム一覧

Field(カラム名), Type(型), Null(Nullable か), Key(キー設定), Default(デフォルト値), Extra(カラムの追加情報)が見れる.

```
show fields from (databasename).(tablename);
```

###### 例

```
mysql> SELECT * FROM world.city
    -> WHERE CountryCode="JPN" AND District="Tokyo-to"
    -> ORDER BY Population DESC;
```

| ID   | Name            | CountryCode | District | Population |
| ---- | --------------- | ----------- | -------- | ---------: |
| 1532 | Tokyo           | JPN         | Tokyo-to |    7980230 |
| 1553 | Hachioji        | JPN         | Tokyo-to |     513451 |
| 1578 | Machida         | JPN         | Tokyo-to |     364197 |
| 1626 | Fuchu           | JPN         | Tokyo-to |     220576 |
| 1636 | Chofu           | JPN         | Tokyo-to |     201585 |
| 1648 | Kodaira         | JPN         | Tokyo-to |     174984 |
| 1655 | Mitaka          | JPN         | Tokyo-to |     167268 |
| 1657 | Hino            | JPN         | Tokyo-to |     166770 |
| 1664 | Tachikawa       | JPN         | Tokyo-to |     159430 |
| 1674 | Hitachinaka     | JPN         | Tokyo-to |     148006 |
| 1683 | Ome             | JPN         | Tokyo-to |     139216 |
| 1686 | Higashimurayama | JPN         | Tokyo-to |     136970 |
| 1689 | Musashino       | JPN         | Tokyo-to |     134426 |
| 1728 | Higashikurume   | JPN         | Tokyo-to |     111666 |
| 1731 | Koganei         | JPN         | Tokyo-to |     110969 |
| 1740 | Kokubunji       | JPN         | Tokyo-to |     106996 |
| 1741 | Akishima        | JPN         | Tokyo-to |     106914 |
| 1756 | Hoya            | JPN         | Tokyo-to |     100313 |
