#SQL知识大集合

记录工作学习过程中运用到的sql知识。

##SQL连接查询

- `join`        内连接  如果表中有至少一个匹配，则返回行  
- `left join `  左连接  即使右表中没有匹配，也从左表返回所有的行  
- `right join`  右连接  即使左表中没有匹配，也从右表返回所有的行  
- `full join `  全连接  只要其中一个表中存在匹配，就返回行  

```C#
var query = (from s in ctx.Students.Include("ClassRooms")
             join sd in ctx.StudentDescriptions on s.StudentID equals sd.StudentID into g
             from stuDesc in g.DefaultIfEmpty()
             select new
                        {
                            Name=s.StudentName,
                            StudentId=s.StudentID,

             }).SingleOrDefault();
```

    




