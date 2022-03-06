---
title: MySQL
date: 2022-2-9 16:56:43
tags:
  - Java 常用类
categories: Java 博客
---

## 分页查询

### 优势
  - 减少服务器内存压力
  - 提高用户体验

### 思路

前端->后端
传递参数: 
  1. 当前页码(pageIndex):查询第几页的数据 设置默认值
  2. 每页展示的记录个数(pageSize):每页展示多少数据 设置默认值

后端->前端
传递参数:
  1. 总条数(total)
    - 获取方式:查询数据库`select count(1) from 表名` 
  2. 分页查询出来的数据(data)
    - `select * from 表名 limit 开始索引,每页显示条数`
      1. 开始的索引 = (当前页码-1) * 每页显示条数
      2. 每页显示条数,由前端传递
  3. 总页数(pageNum)
    - 获取方式:`pageNum = total % pageSize == 0 ? total / pageSize : total / pageSize + 1`
    - 56/5=5时,需要6页来展示
  4. 当前页码(pageIndex)
    - 前端传递
  5. 每页显示条数(pageSize)
    - 前端传递

分页实体类（PageVo）
``` Java
/*
 * pageNum 页数
 * pageSize 个数
 * pageIndex 当前页码
 * total 总条数
 * data 数据
 */
public class PageVo<T> {
  // 用户列表参数
  private Integer pageNum;
  private Integer pageSize;
  private Integer pageIndex;
  private Integer total;
  private List<T> data;

  public PageVo() {
  }

  public PageVo(Integer pageNum, Integer pageSize, Integer pageIndex, Integer total, List<T> data) {
      this.pageNum = pageNum;
      this.pageSize = pageSize;
      this.pageIndex = pageIndex;
      this.total = total;
      this.data = data;
  }

  public Integer getPageNum() {
      return pageNum;
  }

  public void setPageNum(Integer pageNum) {
      this.pageNum = pageNum;
  }

  public Integer getPageSize() {
      return pageSize;
  }

  public void setPageSize(Integer pageSize) {
      this.pageSize = pageSize;
  }

  public Integer getPageIndex() {
      return pageIndex;
  }

  public void setPageIndex(Integer pageIndex) {
      this.pageIndex = pageIndex;
  }

  public Integer getTotal() {
      return total;
  }

  public void setTotal(Integer total) {
      this.total = total;
  }

  public List<T> getData() {
      return data;
  }

  public void setData(List<T> data) {
      this.data = data;
  }

  @Override
  public String toString() {
      return "PageVo{" +
              "pageNum=" + pageNum +
              ", pageSize=" + pageSize +
              ", pageIndex=" + pageIndex +
              ", total=" + total +
              ", data=" + data +
              '}';
  }
}

```
service层
``` Java
public class UserServiceImpl implements UserService {
  private UserDao userDao = new UserDaoImpl();

  @Override
  public PageVo<User> findByPage(int pageIndex, int pageSize) {
    int total = userDao.selectCount();
    int pageNum = total % pageSize == 0 ? total / pageSize : total / pageSize + 1;
    List<User> users = userDao.findLimit((pageNum - 1) * pageSize, pageSize);
    return new PageVo<User>(pageNum,pageSize,pageIndex,total,users);
  }
}
```
     