# IndexedDB 示例
优势：
1. 异步，不阻塞
2. 多数据类似，不只是字符串，比如二进制文件
3. 事务性
4. 浏览器兼容性还行

使用场景：对于后台、中台项目可以尝试使用

### 安装、运行
* git clone https://github.com/yuelau/indexeddb.git
* cd indexeddb
* npm install -g @vue/cli
* npm install -g @vue/cli-service-global
* npm run serve
* 打开浏览器控制台 （eg: Chrome）
* 找到 `Application` 选项
* 点击 `Storage -> IndexedDB 查看初始化创建的 db_user 数据库` 

### 使用
* 点击 `添加数据` 多加点数据
* 点击 `遍历数据` 出来所有的添加的数据
* 其他功能自行尝试

### 注意
* node 版本 >= 8.9

### 参考
* [w3.org &raquo;](https://www.w3.org/TR/IndexedDB/#key)

* [入门 &raquo;](https://wangdoc.com/javascript/bom/indexeddb.html#indexeddb-%E5%AF%B9%E8%B1%A1)