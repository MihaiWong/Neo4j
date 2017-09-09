[TOC]
# CSV文件导入Neo4j数据库
------------

# 1.文件夹创建
![](https://i.imgur.com/iCkXbDq.png)
	启动Neo4j,进入Database Location安装目录下，新建import文件夹，用于导入csv文件（例如：C:\Users\MC\Documents\Neo4j\default.graphdb）；因为Neo4j默认打开载入目录是从import打开，否则会出现找不到文件的情况

# 2.Excel转换CSV
### 2.1节点文件 Excel 格式
![](https://i.imgur.com/fqib4ol.png)

>注：请严格按照格式输入，若某个单元格不输入数据，会导致之后导入数据库失败，请用空格代替

### 2.2 Excel文件另存为csv格式
>注：请修改文件格式为utf-8,无BOM编码格式，防止出现中文乱码


# 3.CSV节点文件导入Neo4j

### 3.1 CSV节点文件格式（1.csv）
	字段一一对应值
``` json
	id,name,description,Alias
	1,制造企业,1111,2222
	2,所有制,1111,2222
	153,行业,1111,2222
	3,国有独资企业,1111,2222
	4,股份制企业,1111,2222
	5,集体企业,1111,2222
	6,私营企业,1111,2222
	7,国外独资企业,1111,2222
	8,装备制造,1111,2222

```
### 3.2 Neo4j中执行以下命令

```
	LOAD CSV WITH HEADERS  FROM "file:///1.csv" AS line  
	MERGE (p:test{id:line.id,name:line.name,description:line.description,Alias:line.Alias})

```
参数说明：
![](https://i.imgur.com/GiHYzUM.jpg)

效果图：
![](https://i.imgur.com/KywBmNZ.png)

# 4 CSV关系文件导入Neo4j
与第三步同理

### 4.1 CSV关系文件格式（2.csv）

字段一一对应值
```
	from_id,pro1,pro2,to_id
	1,制造企业,所有制,2
	7,制造企业,行业,153
	2,所有制,国有独资企业,3
	3,所有制,股份制企业,4
	4,所有制,集体企业,5
	5,所有制,私营企业,6
	6,所有制,国外独资企业,7

```

	关系文件参数说明：
#### from_id
	关系起点的id

#### pro1,pro2
	关系名称
>注：可以有多个属性

#### to_id
	指向的对象的id

### 4.2 Neo4j中执行以下命令

```
	LOAD CSV WITH HEADERS FROM "file:///2.csv" AS line  
	match (from:test1{id:line.from_id}),(to:test1{id:line.to_id})  
	merge (from)-[r:rel{pro1:line.pro1,pro2:line.pro2}]->(to)

```
参数说明：
![](https://i.imgur.com/V9BrUeJ.png)

效果图：
![](https://i.imgur.com/vzRQBSz.png)



>节点文件和关系文件要依次导入
