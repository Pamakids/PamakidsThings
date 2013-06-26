常用Mongo命令
--------

Command | Result 
--- | ---
show dbs | 显示数据库列表
db | 显示当前使用的库
use databse | 使用`database`库
db.collection.find() | 检索`collection`
db.collection.remove(`query`, `justOne`) <br> db.collection.remove()| 删除某条记录</br>删除所有记录
show collections | 查询所有表
db.dropDatabase | 删除当前数据库
db.getName() | 获取数据库名称
db.collection.getIndexes() | 获取索引列表
db.collection.dropIndex({"name":1}) | 删除索引
db.collection.dropIndexes("*") | 删除所有索引
db.collection.insert({device_id:'test'}) | 插入数据

Query
---
Selectors | Operator | Info
--- | --- | ---
**Comparison** | $all | 检索某Field数组是否有多项值，`$all必须扫描所有匹配检索数组的第一个值的数据` <br> db.inventory.find( { tags: { $all: [ "appliances", "school", "book" ] } } )
| $gt | `G`reater `T`han `>` <br> db.inventory.update( { "carrier.fee": { $gt: 2 } }, { $set: { price: 9.99 } } )
| $gte | `G`reater `T`han and `E`qual `>=` <br> db.inventory.update( { "carrier.fee": { $gte: 2 } }, { $set: { price: 9.99 } } )
| $in | $in selects the documents where the field value equals any value in the specified array <br> db.inventory.update( { tags: { $in: ["appliances", "school"] } }, { $set: { sale:true } }
| $lt | `L`ess `T`han `<`
| $lte | `L`ess `T`han and `E`qual `<=`
| $ne | `N`ot `E`qual
| $nin | `N`ot `In` $nin selects the documents where:<br> * the field value is not in the specified array or <br> * the field does not exist.<br> If the field holds an array, then the $nin operator selects the documents whose field holds an array with no element equal to a value in the specified array
**Logical** | $and | 默认就会是AND <br> db.inventory.find( { price: 1.99, qty: { $lt: 20 } , sale: true } ) <br> db.inventory.update( { price: { $ne: 1.99, $exists: true } } , { $set: { qty: 15 } } )
| $nor | 非或不存在，条件为`数组` <br> db.inventory.find( { $nor: [ { price: 1.99 }, { sale: true } ]  } )<br> db.inventory.find( { $nor: [ { price: 1.99 }, { price: { $exists: false } },{ sale: true }, { sale: { $exists: false } } ] } ) <br> contain the price field whose value is not equal to 1.99 and contain the sale field whose value is not equal to true
| $not | 非或不存在，条件为`单值` <br> db.inventory.find( { price: { $not: { $gt: 1.99 } } } ) <br> db.inventory.find( { item: { $not: `/^p.*/` } } )
| $or | 使用$or的时候是分开检索，这样还能使用各项的索引 <br> db.inventory.find ( { $or: [ { price: 1.99 }, { sale: true } ] } ) <br> 如果加上排序就不能使用索引了 <br> db.inventory.find ( { $or: [ { price: 1.99 }, { sale: true } ] } ).sort({item:1})
**Element** | $exists | 判断Field值是否存在 <br> db.inventory.find( { qty: { $exists: true, $nin: [ 5, 15 ] } } )
| $mod | 求余 <br> db.inventory.find( { qty: { $mod: [ 4, 0 ] } } ) <br> 比使用where更快 <br> db.inventory.find( { $where: "this.qty % 4 == 0" } )
| $type | 判断是什么类型 <br> db.inventory.find( { price: { $type : 1 } } )
**Javascript** | $regex | 使用时需要注意表达式的性能问题，只有使用`^`作为字符串开始符才能使用索引 <br> db.collection.find( { field: { $regex: /acme.\*corp/i, $nin: [ 'acmeblahcorp' ] } ); <br> $regex can only use an index efficiently when the regular expression has an anchor for the beginning (i.e. ^) of a string and is a case-sensitive match. Additionally, while `/^a/, /^a.*/, and /^a.*$/` match equivalent strings, they have different performance characteristics. All of these expressions use an index if an appropriate index exists; however, `/^a.*/, and /^a.*$/` are slower. /^a/ can stop scanning after matching the prefix.
| $where | 尽量避免使用，无法利用索引 <br> db.myCollection.find( { active: true, $where: "this.credits - this.debits < 0" } );
**Array** | $elemMath | 数组里每项匹配 <br> db.collection.find( { array: { $elemMatch: { value1: 1, value2: { $gt: 1 } } } } );
| $size | Queries cannot use indexes for the $size portion of a query <br> db.collection.find( { field: { $size: 2 } } );

Update
----

Type | Operator | Info
--- | --- | ---
**Fields**| $set | 修改值 db.collection.update( { field: value1 }, { $set: { field1: value2 } } );
| $unset | 删除值 db.collection.update( { field: value1 }, { $unset: { field1: "" } } );
| $inc | 增值 db.collection.update( { field: value },{ $inc: { field1: amount } } ); <br> `multi`表示所有db.collection.update( { age: 20 }, { $inc: { age: 1 } }, { multi: true } ); <br>db.collection.update( { name: "John" }, { $inc: { age: 2 } }, { multi: true } );
| $rename | 改变名称 db.students.update( { _id: 1 }, { $rename: { "name.first": "name.fname" } } )
**Array** | $ | 如果不知道grades是80的在数组的哪里，就用`$`，它表示匹配第一个符合条件的对象 db.students.update( { _id: 1, grades: 80 }, { $set: { "grades.$" : 82 } } )<br>db.students.update( { _id: 4, "grades.grade": 85 }, { $set: { "grades.$.std" : 6 } } )
| $addToSet | 向数组添加元素，`不可重复、不保证数组序列` db.collection.update( { field: value }, { $addToSet: { field: value1 } } );
| $pop | 删除数组最后一个元素 db.collection.update( {field: value }, { $pop: { field: `1` } } );<br> 删除数组第一个元素 db.collection.update( {field: value }, { $pop: { field: `-1` } } );
| $pullAll | 删除数组多个元素 db.collection.update( { field: value }, { $pullAll: { field1: [ value1, value2, value3 ] }
| $pull | 移除数组votes里`所有`为7的值 db.profiles.update( { votes: 3 }, { $pull: { votes: 7 } } ) <br> 移除比6大的值 db.profiles.update( { votes: 3 }, { $pull: { votes: { `$gt`: 6 } } } )
| $pushAll | 增加多个值到数组 db.collection.update( { field: value }, { $pushAll: { field1: [ value1, value2, value3 ] } } );
| $push | 增加一个值到数组 db.students.update(query,{$push:{field:`value`}})<br>如果`value`是数组，默认会将数组作为一个元素添加到数组里

	db.students.update( { name: "joe" },
                    { $push: { quizzes: { $each: [ { id: 3, score: 8 },
                                                   { id: 4, score: 7 },
                                                   { id: 5, score: 6 } ],
                                          $sort: { score: 1 },
                                          $slice: -5
                                        }
                             }
                    }
                  )


Mongoose
---
Methods | Code
--- | ---
Model.findOne | `Model.findOne({age:5})` 找到一条记录
Model.findById | `Model.findById(obj.id)` 根据id查找记录
Model.count | `Model.count(conditions, callback)` 计算符合条件的记录数
Model.remove | `Model.remove(conditions, callback)` 删除符合条件的记录
Model.distinct | `Model.distinct(field, conditions, callback)` 获取某field唯一值且符合conditions的记录
Model.where | Model.where('age').gte(25).where('tags').in(['movie','music','art']).select('name', 'age', 'tags').skip(20).limit(10).asc('age').slaveOk().hint({age:1,name:1}).exe(callback)


Population Across Three Collections
---
	var user = new Schema{	//新建Schema对象
		name:String,
		friends:[{
			type:Schema.ObjectId,
			ref: 'User'
		}]
	};
	var User = mongoose.model('User', user);
	
	var blogpost = Schema({		//将Json对象强制转换为Schema
		title:String,
		tags:[String],
		author:{
			type:Schema.ObjectId,
			ref: 'User'
		}
	})
	var BlogPost = mongoose.model('BlogPost', blogpost);
	
	….
	
	BlogPost.find({tags:'fun'})				//检索条件
		 .lean()							//just return plain objects, not documents wrapped by mongoose
		 .limit(2)							//限制最多2条
		 .populate('author')				//将author引用的值填充到结果里
		 .exec(function(err, docs){
		 	var opts = {
		 		path: 'author.friends',		//填充值
		 		select: 'name id',			//选择填充属性
		 		options: {limit:2}			//填充数
		 	}
		 	
		 	BlogPost.populate(docs, opts, function(err, docs){  	
		 		
		 	})
		 })
		 
	Mode.find()
		.populate({
			path: 'author',
			math: {name: 'Username'},
			select: 'name',
			options: {comment:'population'} //开启profile后能看到的备注信息
		})		 
		
API | Method | Example
--- | --- | ---
**Schema** | | new Schema()
| select | true *always be included in the results*<br>false *exculded by default*
| isSelected | doc.isSelected('path') 如果有选中则为true
**Query** | | var query = Modle.find()
| exec 执行查询 | query.exec(); <br> query.exec(callback); <br> query.exec('update'); <br> query.exec('find', callback);
| find 查找documents<br> `无callback不执行` | query.find({ name: 'Los Pollos Hermanos' }).find(callback)
| $where 调用js查询 <br>`优先使用内置查询方法` | query.$where('this.comments.length &gt; 10 || this.name.length &gt; 5')
| where 选择path查询<br>`可联合多path` | User.find({age: {$gte: 21, $lte: 65}}, callback); <br>`could instead of`<br>User.where('age').gte(21).lte(65);<br>`moreover, chain a branch of these`<br>User.where('age').gte(21).lte(65)<br>.where('name', /^b/i)<br>.where('friends').slice(10)<br>.exec(callback)
| select | query.select('a b -c'); `include a and b , exclude c`<br>如果有path是以-号开始也可以使用<br>query.select({a: 1, b: 1, c: 0});
