---
id: 587d7fb2367417b2b2512bf8
title: Get Data from POST Requests
challengeType: 2
forumTopicId: 301511
---

## Description
<section id='description'>
Mount a POST handler at the path <code>/name</code>. It’s the same path as before. We have prepared a form in the html frontpage. It will submit the same data of exercise 10 (Query string). If the body-parser is configured correctly, you should find the parameters in the object <code>req.body</code>. Have a look at the usual library example:
<blockquote>route: POST '/library'<br>urlencoded_body: userId=546&bookId=6754 <br>req.body: {userId: '546', bookId: '6754'}</blockquote>
Respond with the same JSON object as before: <code>{name: 'firstname lastname'}</code>. Test if your endpoint works using the html form we provided in the app frontpage.
Tip: There are several other http methods other than GET and POST. And by convention there is a correspondence between the http verb, and the operation you are going to execute on the server. The conventional mapping is:
POST (sometimes PUT) - Create a new resource using the information sent with the request,
GET - Read an existing resource without modifying it,
PUT or PATCH (sometimes POST) - Update a resource using the data sent,
DELETE => Delete a resource.
There are also a couple of other methods which are used to negotiate a connection with the server. Except from GET, all the other methods listed above can have a payload (i.e. the data into the request body). The body-parser middleware works with these methods as well.
</section>

## Instructions
<section id='instructions'>

</section>

## Tests
<section id='tests'>

```yml
tests:
  - text: 'Test 1 : Your API endpoint should respond with the correct name'
    testString: 'getUserInput => $.post(getUserInput(''url'') + ''/name'', {first: ''Mick'', last: ''Jagger''}).then(data => { assert.equal(data.name, ''Mick Jagger'', ''Test 1: "POST /name" route does not behave as expected'') }, xhr => { throw new Error(xhr.responseText); })'
  - text: 'Test 2 : Your API endpoint should respond with the correct name'
    testString: 'getUserInput => $.post(getUserInput(''url'') + ''/name'', {first: ''Keith'', last: ''Richards''}).then(data => { assert.equal(data.name, ''Keith Richards'', ''Test 2: "POST /name" route does not behave as expected'') }, xhr => { throw new Error(xhr.responseText); })'

```

</section>

## Challenge Seed
<section id='challengeSeed'>

</section>

## Solution
<section id='solution'>

```js
// solution required
```

var express = require('express');
var bodyParser = require('body-parser');

var app = express();


app.get("/", function(req, res) {
        res.sendFile( __dirname + "/views/index.html");
});

app.use(express.static(__dirname + "/public"));

// --> 7)  Mount the Logger middleware here

app.use(function(req, res, next){
  console.log(req.method + ' ' + req.path + ' - ' + req.ip); 
  next();
});

// --> 11)  Mount the body-parser middleware  here

app.use(
  bodyParser.urlencoded(
    { 
      extended: false
    }
  )
);

/** 1) Meet the node console. */
console.log("Hello World");

/** 2) A first working Express Server */


/** 3) Serve an HTML file */


/** 4) Serve static assets  */
app.use(express.static(__dirname + '/public'))

/** 5) serve JSON on a specific route */
/*
app.get("/json", function(req, res) {
        res.json({"message": "Hello World"});
  });
*/

/** 6) Use the .env file to configure the app */
 app.get("/json", function(req,res){
   if(process.env.MESSAGE_STYLE === "uppercase"){
     res.json({
       "message": "HELLO JSON"
     })
   }
   res.json({
     "message": "Hello json"
   })
 })
 
/** 7) Root-level Middleware - A logger */
//  place it before all the routes !


/** 8) Chaining middleware. A Time server */

app.get('/now', function(req,res, next){
  next();
}, function(req, res){
 var time = new Date().toString();
  console.log('time'+time);
  res.json({'time': time});
});

/** 9)  Get input from client - Route parameters */

app.get('/:word/echo', function(req, res){
  res.json({
    echo: req.params.word 
  })
  console.log(req.params.word)  
});

/** 10) Get input from client - Query parameters */
// /name?first=<firstname>&last=<lastname>

app.route('/name').get((req, res) => {
   var first = req.query.first;
   var last = req.query.last;
   var jsonObj = {name: first + ' ' + last};
   res.send(jsonObj);
 }).post();
  
/** 11) Get ready for POST Requests - the `body-parser` */
// place it before all the routes !


/** 12) Get data form POST  */

app.post('/name', (req, res) => {
  // Handle the data in the request
  let name = req.body.first + ' ' + req.body.last;
  res.json({name: name});
});

// This would be part of the basic setup of an Express app
// but to allow FCC to run tests, the server is already active
/** app.listen(process.env.PORT || 3000 ); */

//---------- DO NOT EDIT BELOW THIS LINE --------------------

 module.exports = app;
</section>
