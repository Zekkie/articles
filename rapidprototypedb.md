# Rapid prototyping a database

Modern applications require modern solutions. In a world where applications are generating tons and tons of data a database is a must. Even when using an API, the provider is using a database. I this article I'm going to explain how to setup a database(MySql) using an ORM(object relational mapping) for your prototypes. 

## Relations

Since we're using MySql, relations have to be explained since MySql is a relational database. 

### One to one relation

A one to one relationship, is a relationship where one entity has one entity of an other that only belongs to that one entity. Let me explain. An order(product) has one order detail and the order detail can only belong to one order.

### One to many relation

A one to many relationship is easier to explain. An entity can have more of an entity but the entities can only belong to that one entity. For example, a user can have multiple blog posts, but those blogposts can only belong that spicific user. 

### Many to many relation

Last but not least, the many to many relationship. This is where multiple entities can have ownership over multiple entities and these entities can be owned by multiple of the entities. For example a following system. Let's say we have four users, A B and C. User A can follow user B, but user B can also be followed by user C.


## ORM

An ORM is a system that helps the user to not write their own queries and define tables with codes. In this example we are going to use [Sequelize](http://docs.sequelizejs.com/).

### Model

The model is basicly your database table represented in code. 

```javascript

const userModel = sequelize.define("user",{
	id: {
		type: Sequelize.INTEGER(11),
		unique: true,
		autoIncrement:true
	} 
	name: Sequelize.STRING(200),
	email: Sequelize.STRING(150)
})

```

This is a basic user model, with this we can create, delete, update, retreive user data. We can simply create a user like this.

```javascript
userModel.create({
	name: "Zekkie",
	email: "zekkie@test.nl"
})
```
This will create a single user for you. 

If we want to find this user we can write the code below.

```javascript
userModel.findOne({
	where:{
		id:1
	}
}).then(res => {
	console.log(res)
})

//log

{
	id:1,
	name: "Zekkie",
	email: "zekkie@test.nl"
}

```

### Relations in Sequelize

It's very easy to setup a relation in sequelize. Let's create a post model first.

```javascript
const postModel = sequelize.define("post",{
	id: {
		type: Sequelize.INTEGER(11),
		unique: true,
		autoIncrement:true
	} 
	content: Sequelize.STRING(200),
	userId: Sequelize.INTEGER(11)
})
```
You can see that we have added a row called userId. This is so we can store a reference to the user who created the post. 

Now to make a relation, we have to create an association as called in Sequelize. 

```javascript
userModel.hasMany(postModel,{as:"posts"});
postModel.belongsTo(userModel);
```

Let's create a sample blogpost.

```javascript
postModel.create({
	content: "This is sample content",
	userId: 1
})
```
Now we have created a post that belongs to user with the id of 1. And now if we want to get the user and all of his/her posts. We can write the following.
```javascript
userMode.findOne({
	where: {
		id:1
	},
	include:[
		{
			model: postModel,
			as: "posts"
		}
	]
}).then( result => {
	console.log(result)
});


//log

{
	id:1,
	name: "Zekkie",
	email: "zekkie@test.nl",
	posts: [
		id:1,
		content: "This is sample content"
	]
}
```

## Conclusion

As you can see, we have create a simple relational database without writing a single SQL query. This is very powerful, because now you are able to create a prototype backed with a database without having to worry much about SQL.