---
layout: post
title: 储存过程和存储函数
date: 2018-08-28 00:00:00
tags: 数据库
---
[慕课网学习笔记][1]
### 储存过程和存储函数
指存储在数据库中供所有用户程序调用的子程序叫存储过程、存储函数

相同点：完成特定功能的程序
区别：是否用return语句返回值

调用存储过程：

```
1）exec sayHelloWorld();
2)begin
    sayHelloWorld();
    sayHelloWorld();
   end;
   /
set serveroutput on;打开输出才会显示在屏幕上
```

```
create or replace procedure sayHelloWorld
as
--说明部分
begin
  dbms_output.put_line('Hello world');
end sayHelloWorld;
```

```
--创建一个带参数的存储过程：
--给指定的员工涨100块钱的工资，并且打印涨前和涨后的薪水
create or replace procedure raiseSalary(eno in number)
as
--定义一个变量保存涨前的薪水
psal emp.sal%type;
begin
--得到员工涨前的薪水
    sleect sal into psal from emp where empno = eno;
--给该员工涨100
    update emp set sal = sal + 100 where empno = eno;
--需不需要commit？
--注意：一般不在存储过程或者存储函数中提交和回滚。
    dbms_output.put_line('涨前：'||psal||'涨后：'(psal+100));
end;
/
```
```
存储函数：查询某个员工的年收入
create or replace function queryempincome(eno in number)
return number
as
--定义变量保存员工的薪水和奖金
psal emp.sal%type;
pcomm emp.comm%type;
begin
--得到该员工的月薪和奖金
    select sal,comm into psal,pcomm from emp where empno = eno;
--直接返回年收入
    return psal*12+nvl(pcomm,0);
end;
/
```

过程和函数都可以通过out指定一个或多个输出参数，我们可以利用out参数，在过程和函数中实现返回多个值
原则：如果只有一个返回值，用存储函数；否则，就用存储过程。

```
--out参数：查询某个员工姓名 月薪和职位
create or replace procedure queryempInfom(eno in number,
                                        pename out varchar2,
                                        psal out number,
                                        pjob out varchar2)
as
begin
--得到该员工的姓名 月薪和职位
    select ename,sal,empjob into pname,psal,pjob from emp where empno = eno;
end;
/
```

思考：1.查询某个员工的所有信息 --- > out参数太多？
2.查询某个部门中所有员工的所有信息 --->  out中返回集合？

### 在out参数中使用光标
申明包结构:包头+包体

```
--包头
create or replace package mypackage as
    type empcursor is ref cursor;
    prodecure queryEmpList(eno in number,empList out empcursor);
end mypackage;

--包体
create or replace package mypackage as
    type empcursor is ref cursor;
    prodecure queryEmpList(dno in number,empList out empcursor)
    as
    begin
--打开光标
        open empList for select * from emp where deptno = dno;
    end queryEmpList;
end mypackage;
```

```
public class TestCursor{
    @Test
    public void testCurdor(){
        String sql = "{call MYPACKAGE.queryEmpList(?,？)}";
        Connection conn = JDBCUtils.getConnection();
        CallableStatement call = conn.prepareCall(sql);
        //对于in参数赋值
        call.setInt(1,10);
        //对于out参数申明
        call.registerOutParameter(2,OracleTypes.CURSOR);
        //执行调用
        call.execute();
        //取出该部门中所有员工的信息
        ResultSet rs = ((OraclCallableStatement)call).getCursor(2);
        while(rs.next()){
            int empno = rs.getInt("empno");
            ...
        }
    }
}
```


  [1]: https://www.imooc.com/learn/370