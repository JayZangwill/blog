---
title: 常用的mongodb命令
date: 2017-04-10 18:57:18
tags: [mongodb]
---

# 下面是一些我常用的mongodb命令，供自己备忘


<!-- more -->

1. show dbs （列出所有数据库）
2. use [database name] （数据库的切换）
3. show collections （查看当前使用数据库有哪些表）
4. db.[表名].find() （查找当前表下所有数据）
5. db.[表名].findOne({'key':value}) （查找一条符合查询条件的数据）
6. db.[表名].drop() （删除当前表）
7. db.[表名].move({'key':value}) （根据条件删除数据）
8. help （输出mongodb的帮助）
9. db help() （数据库的帮助命令）

[关于mongodb更多](http://blog.csdn.net/u010305706/article/details/48129131)

# node与mongodb交互
```node
// 插入操作
function insertDocuments(db, data, callback) {
	let collection = db.collection('documents');
	collection.insertOne(data, (err, result) => {
		assert.equal(err, null);
		callback(result);
	});
}
```
```node
// 查找操作
function findDocuments(db, callback, search) {
	search = search || {};
	
	// Get the documents collection
	let collection = db.collection('documents');
	// Find some documents
	collection.find(search).toArray((err, docs) => {
		assert.equal(err, null);
		callback(docs);
	});
}
```

```node
// 更新操作
function updateDocument(db, userinfo, money, callback) {

	// Get the documents collection
	let collection = db.collection('documents');

	collection.updateOne({
		idCard: userinfo
	}, {
		$set: {
			money: money
		}
	}, (err, result) => {
		assert.equal(err, null);
		callback(result);
	});
}
```
```node
function deleteDocument(db, userinfo, callback) {
  // Get the documents collection
  let collection = db.collection('documents');
  collection.deleteOne({ idCard: userinfo }, (err, result) => {
    callback(result);
  });
}
```
[更多node与mongodb交互](https://github.com/mongodb/node-mongodb-native)
