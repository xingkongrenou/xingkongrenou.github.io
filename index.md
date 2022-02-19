34道MySQL作业题目

**1、取得每个部门最高薪水的人员名称**

```sql
#第一步：查找出每个部门最高的薪水
select max(sal),deptno from emp group by deptno;
+----------+--------+
| max(sal) | deptno |
+----------+--------+
|  5000.00 |     10 |
|  3000.00 |     20 |
|  2850.00 |     30 |
+----------+--------+
#第二步：将薪水，部门和姓名匹配上
select e.ename,t.* from emp e join (select deptno,max(sal) as maxsal from emp group by deptno) t on t.deptno = e.deptno and t.maxsal = e.sal;
+-------+--------+---------+
| ename | deptno | maxsal  |
+-------+--------+---------+
| BLAKE |     30 | 2850.00 |
| SCOTT |     20 | 3000.00 |
| KING  |     10 | 5000.00 |
| FORD  |     20 | 3000.00 |
+-------+--------+---------+
```

**2、哪些人的薪水在部门的平均薪水之上**

```sql
#第一步：查找出每个部门的平均薪水
select avg(sal),deptno from emp group by deptno;
+-------------+--------+
| avg(sal)    | deptno |
+-------------+--------+
| 2916.666667 |     10 |
| 2175.000000 |     20 |
| 1566.666667 |     30 |
+-------------+--------+
#第二步：找出每个部门高于自己部门平均薪水的员工姓名
select e.ename,e.sal,t.* from emp e join (select deptno,avg(sal) as avgsal from emp group by deptno) t on e.deptno = t.deptno and e.sal > t.avgsal;
+-------+---------+--------+-------------+
| ename | sal     | deptno | avgsal      |
+-------+---------+--------+-------------+
| ALLEN | 1600.00 |     30 | 1566.666667 |
| JONES | 2975.00 |     20 | 2175.000000 |
| BLAKE | 2850.00 |     30 | 1566.666667 |
| SCOTT | 3000.00 |     20 | 2175.000000 |
| KING  | 5000.00 |     10 | 2916.666667 |
| FORD  | 3000.00 |     20 | 2175.000000 |
+-------+---------+--------+-------------+
```

**、取得部门中（所有人的）平均的薪水等级**

```sql
#第一步：获取所有的的薪水等级
select e.ename,e.sal,e.deptno,s.grade from emp e join salgrade s on e.sal between s.losal and s.hisal;
+--------+---------+--------+-------+
| ename  | sal     | deptno | grade |
+--------+---------+--------+-------+
| SMITH  |  800.00 |     20 |     1 |
| ALLEN  | 1600.00 |     30 |     3 |
| WARD   | 1250.00 |     30 |     2 |
| JONES  | 2975.00 |     20 |     4 |
| MARTIN | 1250.00 |     30 |     2 |
| BLAKE  | 2850.00 |     30 |     4 |
| CLARK  | 2450.00 |     10 |     4 |
| SCOTT  | 3000.00 |     20 |     4 |
| KING   | 5000.00 |     10 |     5 |
| TURNER | 1500.00 |     30 |     3 |
| ADAMS  | 1100.00 |     20 |     1 |
| JAMES  |  950.00 |     30 |     1 |
| FORD   | 3000.00 |     20 |     4 |
| MILLER | 1300.00 |     10 |     2 |
+--------+---------+--------+-------+
#第二步：将以上的结果进行按照deptno分组，求grade的平局值
select e.ename,e.sal,e.deptno,avg(s.grade) from emp e join salgrade s on e.sal between s.losal and s.hisal group by deptno;
+-------+---------+--------+--------------+
| ename | sal     | deptno | avg(s.grade) |
+-------+---------+--------+--------------+
| CLARK | 2450.00 |     10 |       3.6667 |
| SMITH |  800.00 |     20 |       2.8000 |
| ALLEN | 1600.00 |     30 |       2.5000 |
+-------+---------+--------+--------------+
```

**4、不准用组函数（Max ），取得最高薪水**

```sql
#将薪水自上而下排序，然后只取第一个值
select ename,sal from emp order by sal desc limit 1;
```

