Mongous
==========
Mongous 是一个轻量且高效的 jQuery 语法风格的 MongoDB 驱动。

### 如何工作

    var $ = require("mongous").Mongous;

    $("database.collection").save({my:"value"});

    $("database.collection").find({},function(r){
        console.log(r);
    });

完成。Mongous 使得你能轻而易举地在代码中进行存储，远离讨厌的数据库连接、集合和嵌套回调函数。

### 数据库 & 集合

- <code>db('Database.Collection')</code>
    - Database      是数据库的名称
    - Collection    是集合的名称
    - 示例
        - <code>db('blog.post')</code>
        - <code>db('blog.post.body')</code>

### 命令

- **更新** <code>db('blog.post').update(find, update, ...)</code>
    - find      是你要查找的对象
    - update    是你要用于更新的对象
    - ...
        - <code>{ upsert: true, multi: false }</code>
        - <code> true, true </code>
- **保存** <code>db('blog.post').save(what)</code>
    - what      是用于更新或创建的对象
- **插入** <code>db('blog.post').insert(what...)</code>
    - what      是用于创建的对象或包含用于创建的对象的数组
    - 示例
        - <code>db('blog.post').save({hello: 'world'})</code>
        - <code>db('blog.post').save([{hello: 'world'}, {foo: 'bar'}])</code>
        - <code>db('blog.post').save({hello: 'world'}, {foo: 'bar'})</code>
- **删除** <code>db('blog.post').remove(what, ...)</code>
    - what      是将要被删除的对象
    - ...   true    应用原子性
- **查找** <code>db('blog.users').find(..., function(reply){ })</code>
    - reply     是 MongoDB 的返回对象
    - reply.documents   是从 MongoDB 中检索到的文档数组
    - ...       用于过滤结果的参数
        - 按对象过滤
            - 第一个参数 是你要查找的对象
            - 第二个参数 是你需要返回的字段    
                示例: <code>{ name: 1, age: 1 }</code>
            - 第三个参数 是下列选项中的一些:    
                <code>{ lim: x, skip: y, sort:{age: 1} }</code>
        - 按数字过滤
            - 第一个数字 是返回结果限制量（未设置的话就返回所有结果）
            - 第二个数字 是忽略的文档数
    - 示例
        - <code>db('blog.users').find(5, function(reply){ })</code>    
            reply.documents 包含前 5 个文档，
        - <code>db('blog.users').find(5, {age: 23}, function(reply){ })</code>    
            过滤条件为“age 为 23”，
        - <code>db('blog.users').find({age: 27}, 5, {name: 1}, function(reply){ })</code>    
            只读取 name 字段。
        - <code>db('blog.users').find(5, {age: 27}, {name: 1}, {lim: 10}, function(reply){ })</code>    
            与前面的例子等价，除了限制结果数量为 10 而不是 5
        - <code>db('blog.users').find(5, function(reply){ }, 2)</code>    
            reply.documents 忽略前 2 个文档，包含接下来的 3 个文档。
        - <code>db('blog.users').find(function(reply){ }, {age: 25}, {}, {limit: 5, skip: 2})</code><br/>
            与前面的例子等价，除了过滤条件为“age 为 25”，
        - <code>db('blog.users').find({}, {}, {sort: {age: -1}}, function(reply){ })</code>    
            reply.documents 被按 age 降序排序（顺序排列: {age:1}）。
- **操作** <code>db('blog.$cmd').find(command,1)</code>
    - command   是你要执行的数据库操作命令
    - 示例 <code>db('blog.$cmd').find({drop:"users"},1)</code>    
        删除 users 集合
- **认证** <code>db('blog.$cmd').auth(username,password,callback)</code>
    - username, password    “blog” 数据库的用户名、密码
    - callback              认证结束后执行的回调函数
    - 示例 <code>db('blog.$cmd').auth('user','pass',function(reply){})</code>    
- **打开连接** <code>db().open(host,port)</code>
    - 只有在连接不同的主机和端口的时候才需要手动执行，否则它会自动惰性连接。
            
Mongous 是对 node-mongodb-driver 的精简（少即是多），由 Christian Kvalheim 编写。