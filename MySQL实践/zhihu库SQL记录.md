# SQL记录



1. 统计知乎用户所在城市数量排名前50的城市：

```mysql
select count(*) as cnt, location from user where location is not null group by location order by cnt desc limit 50;
```



2. 统计知乎用户从事行业排名前50的行业：

```mysql
select count(*) as cnt, business from user where business is not null group by business order by cnt desc limit 50;
```



3. 统计知乎用户工作公司排名前50的公司：

```mysql
select count(*) as cnt, employment from user where employment is not null group by employment order by cnt desc limit 20;
```



4. 统计知乎用户毕业大学数量排名前20的大学：

```mysql
select count(*) as cnt, education from user where education is not null group by education order by cnt desc limit 20;
```























