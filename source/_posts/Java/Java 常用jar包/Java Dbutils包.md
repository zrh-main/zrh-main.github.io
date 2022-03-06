---
title: Dbutils包
date: 2022-01-03 19:49:57
tags:
  - Java jar包
categories:
  Java
---

## 概述
DBUtils简化了JDBC的开发步骤，使得我们可以用更少量的代码实现连接数据库的功能


## 核心功能
QueryRunner中提供对sql语句操作的API.
ResultSetHandler接口，用于定义select操作后，封装结果集.
DbUtils类是一个工具类，定义了关闭资源与事务处理的方法
## 作用
  - 简化数据的封装


## QueryRunner

### 构造函数
 `public QueryRunner(DataSource ds)` :传入参数为连接池

### 常用方法
`update(String sql, Object… params)` 执行insert update delete操作
`query(String sql, ResultSetHandler rsh, Object… params)` 执行 select操作


## ResultSetHandler
![](ResultSetHandler结果集处理类.jpg)



## 案例

``` Java
public class EmpDaoImpl implements EmpDao {
    private QueryRunner queryRunner = new QueryRunner(JdbcUtil.getDataSource());
    //  建表
    @Override
    public int createTable() {
        String sql = "CREATE  TABLE  IF NOT EXISTS medicine (kid INT UNSIGNED AUTO_INCREMENT COMMENT '科室编号',kname VARCHAR(15) NOT NULL COMMENT '科室名称',PRIMARY KEY(kid))ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COMMENT = '科室表'";
        try {
            return queryRunner.update(sql);
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return 0;
    }
    //  添加
    @Override
    public int addEmp(Emp emp) {
        try {
            return queryRunner.update("insert into emp (id,ename,job_id,mgr,joindate,salary,bonus,dept_id) values (?,?,?,?,?,?,?,?)",emp.getId(),emp.getEname(),emp.getJobId(),emp.getMgr(),emp.getJoinDate(),emp.getSalary(),emp.getBonus(),emp.getDeptId());
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return 0;
    }
    //  修改
    @Override
    public int updateEmp(Emp emp) {
        try {
            return queryRunner.update("update emp set ename=?,job_id=?,mgr=?,joindate=?,salary=?,bonus=?,dept_id=? where id = ?",emp.getEname(),emp.getJobId(),emp.getMgr(),emp.getJoinDate(),emp.getSalary(),emp.getBonus(),emp.getDeptId(),emp.getId());
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return 0;
    }
    //  删除
    @Override
    public int deleteEmp(int id) {
        try {
            return queryRunner.update("delete from emp where id = ?",id);
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return 0;
    }
    //  查询单个
    @Override
    public Emp selectEmp(int id) {
        try {
            return queryRunner.query("select id,ename,job_id,mgr,joindate,salary,bonus,dept_id deptId from emp where id = ?",new BeanHandler<>(Emp.class),id);
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return null;
    }
    //  查询全部
    @Override
    public List<Emp> selectAllEmp() {
        try {
            return  queryRunner.query("select id,ename,job_id,mgr,joindate,salary,bonus,dept_id deptId from emp ",new BeanListHandler<>(Emp.class));
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return null;
    }
    //  查询个数
    @Override
    public int selectCount(String name) {
        try {
            Object obj = queryRunner.query("select count(1)  from emp where ename like ? ", new ScalarHandler(),"%"+name+"%");
            return Integer.parseInt(String.valueOf(obj));
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return 0;
    }
}
```