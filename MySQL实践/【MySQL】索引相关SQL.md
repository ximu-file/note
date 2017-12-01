### 查看某个表的索引

show index from table_name;
show keys from table_name;

### 创建索引

ALTER TABLE table_name ADD INDEX index_name (column_list，column_list2 )
ALTER TABLE table_name ADD UNIQUE (column_list)
ALTER TABLE table_name ADD PRIMARY KEY (column_list)


CREATE INDEX index_name ON table_name (column_list)
CREATE UNIQUE INDEX index_name ON table_name (column_list)

###删除索引
DROP INDEX index_name ON table_name
ALTER TABLE table_name DROP INDEX index_name
ALTER TABLE table_name DROP PRIMARY KEY
