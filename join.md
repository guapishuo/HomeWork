### 查询广东省所有的省市区号。
    select x.id,p.cityName,s.cityName,x.cityName from s_provinces x --查询s_provinces表中区的id,省名，市名,区名 
        join s_provinces s on x.parentId = s.id         --join on 进行引用把市的id和区的parentId进行关联
        left join s_provinces p on s.parentId = p.id    --left join 关键字会从市的id那返回所有的行，即在p.id中没有匹配的行
    where p.cityName='广东省'   --条件城市名字='广东省'
    
    union all   --操作符用于合并两个或多个 SELECT 语句的结果集
   
    select s.id,p.cityName,s.cityName,null from s_provinces s --查询s_provinces表中市的id，省名，市名，区为null的值
        join s_provinces p on  s.parentId = p.id    
    where p.cityName = '广东省';

#### join on查询出给定条件的数据
#### left jion 左外链接  例: 省表id与市表id相关联，如果市表没有相关联的省市数据则对应null；
    
	广东省   ----- 珠海市,广州市
	广西省   ----- null
	
#### right jion 右外链接 与左外链接刚好相反

	广东省   ----- 珠海市,广州市
	广西省   ----- null
	null       ----- 南昌市			