### 1. 分页

```java
@Select({"select * from", TABLE_NAME, "where datetime<=(select datetime from", TABLE_NAME,"order by datetime desc limit #{firstIdOfPage},1) limit #{pageSize}"})
```

描述：以时间进行倒序查找，然后取出排序后的第 firstIdOfPage 条语句（即第pageNumber 页的第一条语句，但是存在分页展示重叠的问题，pageSize表示页面尺寸。

------



### 2.mybatis查询语句中带有List，通常用在sql语句中带有 in () 操作的语句

```java
@Select(       
	"<script>"        
	+ "select distinct question.id from question,direction where question.id = direction.question and question.difficulty = #{difficulty} and question.type in "        
	+ "<foreach item='item' index='index' collection='typeList' open='(' separator=',' close=')'>"        
	+ "#{item}"        
	+ "</foreach>"        
	+ "and direction.direction in "        
	+ "<foreach item='item1' index='index' collection='directionList' open='(' separator=',' close=')'>"        
	+ "#{item1}"        
	+ "</foreach>"        
	+ "</script>")
```

```java
getCollectByTypeDifficultDirection(@Param("typeList")List<String> typeList, 											  @Param("difficulty")int difficulty,                                                		@Param("directionList") List<String> directionList);
```

附加：distinct 语句用于区分重复的元素

上述 sql 语句改进：

```sql
select question.id from question where difficulty=3 and question.type in ("填空题") and id in (select distinct direction.question from direction where direction.direction in ("java","c"));
```

