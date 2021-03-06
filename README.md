# ORM
[![Latest NPM release](https://img.shields.io/npm/v/@lct/orm.svg)](https://www.npmjs.com/package/@lct/orm)

## 介绍
本项目为基于`typescript`的一个ORM规范。对`Model`做一个约定，提供DB访问ORM接口。
> 接口：[https://github.com/jiamao/orm/blob/master/api.md](https://github.com/jiamao/orm/blob/master/api.md)
## 快速上手
#### 安装
```javascript
npm install @lct/orm
```

#### 定义Model
Model我们认为是跟表一一对应的类。
```javascript
import { BaseModel } from "@lct/orm";

@BaseModel.Table('t_user') //关联表t_user,
//@BaseModel.Table(tablename, ['id'])//可以从这里指定多个主健，也可以在属性中去指定
class MyModel extends BaseModel {  
    @BaseModel.TableField('Fid') //映射属性跟字段, 这里可以不指定这句，默认为关联上Fid
    @BaseModel.TablePrimaryKey() //指定当前属性为唯一健
    id: number = 0;
    @BaseModel.TableField('Fname')
    name: string = "";
    nickName: string = "";
    createTime: string = "";
}
```
#### 使用ORM操作DB
```javascript
import * as mysql from "mysql";
import { BaseModel, DBHelper } from "@lct/orm";
//本地测试数据库
//先准备好测试DB
const connection = mysql.createConnection({
    host     : 'localhost',
    user     : 'jiamao',
    password : '123456',
    database : 'test',
    charset  : 'utf8'
  });

  //实例化DB操作
const db = new DBHelper(connection);
insert();

//新增一个user
async insert() {
    connection.connect();

    let m = new MyModel();
    m.name = "my name";
    m.nickName = "name";
    let ret = await db.insert(m); 
    console.log(ret);
    
    connection.end();
}
```

#### 测试用例
 测试用例是基于mocha的，依赖`mocha`和`assert`。 <br />
 表结构参见`test/t_user.sql`，更新`test/index.ts`中的数据库连接信息。  <br />
 装上依赖后直接执行以下命令即可。 <br />
```
npm run test
```