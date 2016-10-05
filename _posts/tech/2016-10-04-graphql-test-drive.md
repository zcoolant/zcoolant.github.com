---
layout: post
category : 技术笔记
tagline: "Supporting tagline"
tags : [graphql, dev]
---

## Key Idea
Graphql是facebook在2012年开源的一个REST API的替代品，让用户可以用一个HTTP request去完成复杂的对nested数据的查询。最大的惊喜在于，它可以仅返回你需要的数据
> GraphQL is a query language for APIs and a runtime for fulfilling those queries with your existing data. GraphQL provides a complete and understandable description of the data in your API, gives clients the power to ask for exactly what they need and nothing more, makes it easier to evolve APIs over time, and enables powerful developer tools.


## Code

### Schema
graphql有自己的一套type system，nested的type通过reference来实现。这里我们可以demoralize在关系型数据库里标准化的数据

    type Student {
      id: ID!
      name: String
      classes: [Appointment]
    }
    
    type Class {
      id: ID!
      datetime: String
      room: Room
    }
    
    type Room {
      id: ID!
      size: Int
    }
    
    type Query {
      getStudent(id: ID!): Student
    }
    
特殊的type `Query`相当于制定了API的入口，来规定我们可以如何得到Student数据。

### Resolver
graphql只是简化了query的API，但是如何获取数据是需要用户来实现的，简单来说，我们需要定义入口（root）函数，以及每个field的回调函数来最终得到需要的数据。这个函数可以是常量，普通function或者Promise。

    module.exports = {
      getStudent: function ({ID}) {
        return new Student(ID);
      }
    }
    
    class Student {
      construct(id) {
        this.id = id;
        this.name = "Tong Zhao";
      }
      classes(): {
        return [1,2,3].map(x => new Class(x));
      }
      ...
    }

### App.js
最后我们只需要用官方的tool配合express启动我们的server！root就是我们之前定义的对query type的resolver

    var express = require('express');
    var graphqlHTTP = require('express-graphql');
    var { buildSchema } = require('graphql');
    var root = require('./resolver.js');
    var fs = require('fs');
    
    fs.readFile('schema.gql', 'utf8', function (err, data) {
    
      var schema = buildSchema(data);
    
      var app = express();
      app.use('/graphql', graphqlHTTP({
        schema: schema,
        rootValue: root,
        graphiql: true
      }));
    
      app.listen(4000, () => {
        console.log('Running a GraphQL API server at localhost:4000/graphql');
      });
    });

## 总结
