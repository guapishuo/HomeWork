## 城市分割
### 第一种方法 用union连接方法  
    create table lagou_city as
            select
                   d.id,    -                   -省市区编号id
                   p.cityName as province,      --省名
                   c.cityName as city,          --市名
                   d.cityName as district from  --区名
                  (select * from s_provinces where depth=3) d --查询省市表（s_provinces）里的depth=3的数据(3表示第3级也就是区名id)
            join s_provinces c on d.parentId = c.id and c.depth=2 --join把 市名id和省市表的depth的第2级(市名id)连接起来
            join s_provinces p on c.parentId = p.id and p.depth=1 --连接省名id和省市表的depth的第1级(省名id)连接
    
    union --操作符用于合并两个或多个 SELECT 语句的结果集

          select
                 c.id, 
                 p.cityName as province,
                 c.cityName as city,
                 null as district from --查询出省市名 但区名为空的
                 (select * from s_provinces where depth=2) c
          join s_provinces p on c.parentId = p.id and p.depth = 1;
    
### 第二种分布创建
#### 首先把一些基本的，好筛选出来的数据，先提取出来放到我们创建的表里        
       create table lagou_city01 as
       select d.id, p.cityName as province, c.cityName as city, d.cityName as district from
         (select * from china_city where depth=3) d
           join china_city c on d.parentId = c.id and c.depth=2
           join china_city p on c.parentId = p.id and p.depth=1;
#### 先可以把所有‘省，市，县’都有数据的行，从s_provinces中提取到 lagou_city表中，之后我们再把那些县为null的行数添加进去，        
       insert into lagou_city01
       select c.id, p.cityName as province, c.cityName as city, null as district from (select * from china_city where depth=2) c
         join china_city p on c.parentId = p.id and p.depth = 1;
       
### 我认为第二种方法比较简便，先筛选好数据，然后创建数据库，最后再给出要添加的内容的条件添加进去。







