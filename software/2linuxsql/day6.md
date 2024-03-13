# TPShop部署，数据库验证

    -- 1、 列出总人数大于4的部门号和总人数。
    -- 部门人数 > 4
    -- 分组，以 部门号 分组，分完组，需要过滤， > 4
    -- 显示哪些字段 部门号， 总人数
    select deptid, count(*) from employees group by deptid having count(*) > 4;
    -- 2、 列出开发部和和测试部的职工号、 姓名。
    -- 有2个表，部门表有开发部和和测试部， 员工表有职工号、 姓名
    -- 部门表departments 和 员工表employees 如何关联 ===》 部门号 deptid
    -- a) 先查 部门表departments中 开发部和和测试部 对应的部门id
    -- b) 员工表再和上面关联
    select deptid from departments where deptname in ('开发部', '测试部');
    -- 员工表再和子查询的结果关联
    select * from employees as emp inner join (
    select deptid from departments where deptname in ('开发部', '测试部')
    ) as dept on dept.deptid=emp.deptid;
    select emp.empid, emp.empname from employees as emp inner join (
    select deptid from departments where deptname in ('开发部', '测试部')
    ) as dept on dept.deptid=emp.deptid;
    -- 3、 求出各部门党员的人数， 要求显示部门名称。
    -- 各部门， 分组 group by 部门号
    -- 先 过滤 (员工表employees)， 再分组
    -- 要求显示部门名称， 部门名称在部门表departments
    -- a. 员工表employees和部门表departments关联
    select emp.deptid, dept.deptname, count(*) from employees as emp
    inner join departments as dept on dept.deptid=emp.deptid
    where emp.politicalstatus='党员' -- 内连接时，只连接党员
    group by emp.deptid -- 再来分组， having 过滤的内容一定是分组后的字段内容
    -- 4、 列出市场部的所有女职工的姓名和政治面貌。
    select * from employees as emp
    inner join departments as dept on dept.deptid=emp.deptid;
    select * from employees as emp
    inner join departments as dept on dept.deptid=emp.deptid
    where dept.deptname='市场部' and emp.sex='女';
    -- 5、 显示所有职工的姓名、 部门名和工资数。
    select * from employees as emp
    inner join departments as dept on emp.deptid=dept.deptid
    inner join salary as sa on sa.empid=emp.empid
    select emp.empname, dept.deptname, sa.salary from employees as emp
    inner join departments as dept on emp.deptid=dept.deptid
    inner join salary as sa on sa.empid=emp.empid
    -- 6、 显示各部门名和该部门的职工平均工资。
    -- 分组后，显示的是分组的字段，或聚合函数
    select dept.deptid, dept.deptname, avg(sa.salary) from employees as emp
    inner join departments as dept on emp.deptid=dept.deptid
    inner join salary as sa on sa.empid=emp.empid
    group by emp.deptid
    -- 7、 显示工资最高的前3名职工的职工号和姓名。
    select * from employees as emp
    inner join departments as dept on emp.deptid=dept.deptid
    inner join salary as sa on sa.empid=emp.empid
    order by sa.salary desc -- 降序
    limit 3
    select emp.empid, emp.empname, sa.salary from employees as emp
    inner join departments as dept on emp.deptid=dept.deptid
    inner join salary as sa on sa.empid=emp.empid
    order by sa.salary desc -- 降序
    limit 3
    -- 8、 列出工资在1000－2000之间的所有职工姓名。
    select emp.empid, emp.empname, sa.salary from employees as emp
    inner join departments as dept on emp.deptid=dept.deptid
    inner join salary as sa on sa.empid=emp.empid
    where sa.salary between 1000 and 2000
    -- 9、 列出工资比王昭君高的员工。
    -- 找王昭君工资，这个是子查询
    select sa.salary from employees as emp inner join salary as sa
    on sa.empid=emp.empid where emp.empname='王昭君'
    select sa.salary from employees as emp
    inner join departments as dept on emp.deptid=dept.deptid
    inner join salary as sa on sa.empid=emp.empid
    where emp.empname='王昭君'
    -- 子查询作为条件
    select * from employees as emp
    inner join salary as sa on sa.empid=emp.empid
    where sa.salary > 3000
    select * from employees as emp
    inner join salary as sa on sa.empid=emp.empid
    where sa.salary > (
    select sa.salary from employees as emp inner join salary as sa
    on sa.empid=emp.empid where emp.empname='王昭君'
    )
    -- 10、 列出每个部门中工资小于本部门平均工资的员工信息。
    -- 本部门平均工资是多少
    -- 分组后，显示分组的字段，或聚合函数
    select emp.deptid, avg(sa.salary) as avg_salary from employees as emp
    inner join salary as sa on sa.empid=emp.empid
    group by emp.deptid
    select * from employees as emp
    inner join salary as sa on sa.empid=emp.empid
    inner join (
    select emp.deptid, avg(sa.salary) as avg_salary from employees as emp
    inner join salary as sa on sa.empid=emp.empid
    group by emp.deptid
    ) as temp on emp.deptid=temp.deptid
    where sa.salary < temp.avg_salary
    select emp.empname, emp.deptid, sa.salary from employees as emp
    inner join salary as sa on sa.empid=emp.empid
    inner join (
    select emp.deptid, avg(sa.salary) as avg_salary from employees as emp
    inner join salary as sa on sa.empid=emp.empid
    group by emp.deptid
    ) as temp on emp.deptid=temp.deptid
    where sa.salary < temp.avg_salary