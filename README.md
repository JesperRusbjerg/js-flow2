Period-2 Node.js, Express + JavaScript Backend Testing, NoSQL, MongoDB and Mongoose![Image](https://lh4.googleusercontent.com/GVf0T8Lk8bTz-0g-_dOSqcaDcwHlbjnOaeS2FM2GIOw-8222O6JHlsCIlGIepMwaUPr01eiNIOt8LoLxHGdtf4o63-12TARI9-CBc4u6A8NwXyfJVLK27B4FdaWCv5elF548aal3)

Note: This description is too big for a single exam-question. It will be divided up into separate questions for the exam

  

Why would you consider a Scripting Language as JavaScript as your Backend Platform?

It is non blocking, instead of having a blocking thread it uses an event loop, making it extremely effiecient and fast, it makes it able to handle many things at once, as whenever it has a async reqeuest, it goes into the event loop and the answer comes whenever its done.

It is also very succicient for creating a backend quickly, you dont have to compile your code all the time before testing it

Also you can write both backend and frontend code in JS, so you can be better at just JS, and not have to worry as much about other languages

Explain Pros & Cons in using Node.js + Express to implement your Backend compared to a strategy using, for example, Java/JAX-RS/Tomcat

Express is open source, it is up and coming, there are lots of updates and smart fixes to problems, that made JAX annoying. For the most part it is just a different implementation to solving the same problem, the other reasons are listed above

It can however be easier to spot errors in Java, due to typechecks etc, and if you need something super CPU heavy java programs perform better than Node, but node is extremely fast at non CPU heavy operations with its event loop and non blocking thread

      Node.js uses a Single Threaded Non-blocking strategy to handle asynchronous task. Explain strategies to implement a Node.js based server architecture that still could take advantage of a multi-core Server.

You use the evnt loop to handle Sync functions, thereby making them async and thereby not blocking the main thread, as JS always only runs in a single thread. Simply said, API’s solves the problem, for example: Node can install a fetch module that makes fetching an async operation. File readings etc. are all built into node.

Explain briefly how to deploy a Node/Express application including how to solve the following deployment problems:

Previously we would deploy on TOMCAT, that accepts war-files, pre-compiled and good to go. This is not the case with Node, in our case we would upload the files to our nginx server and start a non-dev version of the server, and then our last job is to point all trafic matching the url to the right node project on our nginx server.

- Ensure that you Node-process restarts after a (potential) exception that closed the application 

There are tools for this, such as pm2

- Ensure that you Node-process restarts after a server (Ubuntu) restart 

I think this is also s olved by pm2

-       Ensure that you can take advantage of a multi-core system 

Understanding the eventloop makes this possible, you use either imported or built in APIs to make this possible

- Ensure that you can run “many” node-applications on a single droplet on the same port (80) 

this comes from nginx receiving requests on port 80, you must the config your nginxs locations to direct you to the right project

Explain the difference between “Debug outputs” and application logging. What’s wrong with console.log(..) statements in our backend-code.

Console.log is a blocking function, and it is very easy to forget to delete them

Debug is a tool that runs only in DEBUG/dev mode, and is a non blocking call

Demonstrate a system using application logging and       “coloured” debug statements.

mongoose.connection.on('connected', function () {

debug('Mongoose default connection open ' );

});

So in this scenario, it will only log “Mongoose bla bla” if we set the variable SET DEBUG=minidemo:* when we run the server, if we start it normally, it will log nothing

it is as stated earlier NON BLOCKING which is what you want, and its smart that you do not have to delete them before you go into deploy mode

Explain, using relevant examples, concepts related to testing a REST-API using Node/JavaScript + relevant packages 

ADD RELEVANT EXAMPLE HERE WHEN YOUVE MADE IT IN YOUR ON BACKEND

Rest endpoints can fail, or be unresponsive, which can make tests take a long time. So in this scenario you can use the Nock package to test your end points, where you decide what a given URL should return. 

Explain, using relevant examples, the Express concept; middleware.

Middleware functions are functions that have access to the [request object](https://expressjs.com/en/4x/api.html#req) (req), the [response object](https://expressjs.com/en/4x/api.html#res) (res), and the next middleware function in the application’s request-response cycle. The next middleware function is commonly denoted by a variable named next.

Middleware functions can perform the following tasks:

- Execute any code. 
- Make changes to the request and the response objects. 
- End the request-response cycle. 
- Call the next middleware function in the stack. 

So for example

A middleware might be “Check if restend point is a get request, and save it in the logs”

If it is a get request, log it and call next() if it isnt. just call next()when all middle wares are done, you can get to the route you wish to access

      Explain, using relevant examples, how to implement sessions and the legal implications of doing this.

var cookieSession = require("cookie-session");

app.use(

    cookieSession({

        name: "session",

        secret: "I_should_never_be_visible_in_code",

  

        // Cookie Options

        maxAge: 30*60*1000// 30 minutes

    })

);

  

Recital 30 of the GDPR states:

Natural persons may be associated with online identifiers provided by their devices, applications, tools and protocols, such as internet protocol addresses, cookie identifiers or other identifiers such as radio frequency identification tags.

This may leave traces which, in particular when combined with unique identifiers and other information received by the servers, may be used to create profiles of the natural persons and identify them.

In short: when cookies can identify an individual via their device, it is considered personal data.

Not complying to the laws will result in severe punishment.

  

Compare the express strategy toward (server side) templating with the one you used with Java on second semester.

Both of them use embedded javascript in their HTML code, personally im not a fan

Demonstrate a simple Server Side Rendering example using a technology of your own choice (pug, EJS, ..).

Setting up route

var express = require("express");

var router = express.Router();

router.get("/joke", function(req, res, next) {

    let counter = req.session.jokeCounter;

    counter++;

    req.session.jokeCounter = counter;

    res.render("randomJoke", { title: "Joke", joke: jokes.getRandomJoke() });

});

  

Rendering the site

&lt;!DOCTYPE html&gt;

&lt;html&gt;

    &lt;head&gt;

        &lt;title&gt;&lt;%= title %&gt;&lt;/title&gt;

        &lt;link rel='stylesheet' href='/stylesheets/style.css'/&gt;

    &lt;/head&gt;

    &lt;body&gt;

        &lt;a href='/'&gt;Home Page&lt;/a&gt;

        &lt;p&gt;&lt;%= joke %&gt;&lt;/p&gt;

    &lt;/body&gt;

&lt;/html&gt;

  

Explain, using relevant examples, your strategy for implementing a REST-API with Node/Express and show how you can "test" all the four CRUD operations programmatically using, for example, the Request package.

Implementing a REST-API with Express

var express = require("express");

var router = express.Router();

var jokes = require("../model/jokes");

  

/* GET home page. */

router.get("/", function(req, res, next) {

    res.render("index", { title: "Express", userName: req.session.userName });

});

  

router.get("/login", function(req, res, next) {

    res.render("login", { title: "Login" });

});

  

router.post("/login", function(req, res, next) {

    res.render("index", { title: "Express" });

});

  

router.get("/joke", function(req, res, next) {

    let counter = req.session.jokeCounter;

    counter++;

    req.session.jokeCounter = counter;

    res.render("randomJoke", { title: "Joke", joke: jokes.getRandomJoke() });

});

  

router.get("/jokes", function(req, res, next) {

    let counter = req.session.jokesCounter;

    counter++;

    req.session.jokesCounter = counter;

    res.render("allJokes", { title: "Jokes", jokes: jokes.getAllJokes() });

});

  

router.get("/addjoke", function(req, res, next) {

    res.render("addJoke", { title: "Add Joke" });

});

  

router.post("/storejoke", function(req, res, next) {

    let counter = req.session.storeJokeCounter;

    counter++;

    req.session.storeJokeCounter = counter;

  

    const joke = req.body.joke;

  

    jokes.addJoke(joke);

  

    res.render("addJoke", { title: "Add Joke" });

});

  

module.exports = router;

  

Testing the REST-API

const expect = require("chai").expect;

const http = require('http');

const app = require('../app');

const fetch = require("node-fetch");

const TEST_PORT =3344;

const URL =`http://localhost:${TEST_PORT}/api`;

const jokes = require("../model/jokes");

let server;

describe("Verify the Joke API", function() {

    before(function(done){

        server = http.createServer(app);

        server.listen(TEST_PORT,()=&gt;{

            console.log("Server Started")

            done()

        })

    })

    after(function(done){

        server.close();

        done();

    })

    beforeEach(function(){

        jokes.setJokes(["aaa","bbb","ccc"])

    })

    it("Should add the joke 'ddd",asyncfunction(){

        var init = {

            method: "POST",

            headers : {"content-type": "application/json"},

            body : JSON.stringify({joke: "ddd"})

        }

        await fetch(URL+"/addjoke",init).then(r =&gt; r.json());

        //Verify result

        expect(jokes.getAllJokes()).lengthOf(4);

        expect(jokes.getAllJokes()).to.include("ddd")

    })

}

  
  

Explain, using relevant examples, about testing JavaScript code, relevant packages (Mocha etc.) and how to test asynchronous code.

We can test our code using Mocha and make it easier to see the result using Chai’s expect

      Explain, using relevant examples, different ways to mock out databases, HTTP-request etc.

We can use nock to mock a website

const nock = require('nock');

describe("loadWiki()", function() {

    before(function() {

        //the website to be mocked

        nock("https://en.wikipedia.org")

            //the HTTP method and the path

            .get("/wiki/Abraham_Lincoln")

            //the response the mocked website should send

            .reply(200, "Mock Abraham Lincoln Page");

    });

    it("Load Abraham Lincoln's wikipedia page", function(done) {

        tools.loadWiki({ first: "Abraham", last: "Lincoln"}, function(html) {

            expect(html).to.equal("Mock Abraham Lincoln Page");

            done();

        });

    });

});

  

Explain, preferably using an example, how you have deployed your node/Express applications, and which of the Express Production best practices you have followed.

Deployed on nginx, used logging without blocking and and no sync functions

NoSQL, MongoDB and Mongoose

Explain, generally, what is meant by a NoSQL database.

Non relational data, all data is stored in clusters, no clusters link to eachother, thereby you might find yourself with redundant data, so you have to be smart in your setup. It is very neat if you have a database with lots of reads and not so many writes, It is complicated to write to, as you may have to write in multiple locations. This is easier in a relational world, where often you will just have to switch an ID somewhere, and it is changed everywhere.

Explain Pros & Cons in using a NoSQL database like MongoDB as your data store, compared to a traditional Relational SQL Database like MySQL.

NOSQL: Redundant data, hard to write to. Easy to read from, because data is stored in one place. You dont have to do Joins and look for data in mulitple tables, it is all stored in clusters.

SQL: hard to read, you have to join lots of tables all the time to find the right data. However it is very easy to change data, because things are so split up

Explain reasons to add a layer like Mongoose, on top on of a schema-less database like MongoDB

Mongoose is like JPQL, it is a package that makes it easier to query against a mongoose DB. have not tried to query against it without mongoose

These two topics will be introduced in period-3

- Explain about indexes in MongoDB, how to create them, and demonstrate how you have used them. 
- Explain, using your own code examples, how you have used some of MongoDB's "special" indexes like TTL and 2dsphere 

Demonstrate, using a REST-API you have designed, how to perform all CRUD operations on a MongoDB

done previously in the “jokes section”

Explain the benefits of using Mongoose, and demonstrate, using your own code, an example involving all CRUD operations

done previously in the “jokes section”

Explain the “6 Rules of Thumb: Your Guide Through the Rainbow” as to how and when you would use normalization vs. denormalization.

rule 1 Favor embedding unless there is a compelling reason not to.

rule 2 Needing to access an object on its own is a compelling reason not to embed it.

rule 3 Arrays should not grow without bound. If there are more than a couple of hundred documents on the “many” side, don’t embed them; if there are more than a few thousand documents on the “many” side, don’t use an array of ObjectID references. High-cardinality arrays are a compelling reason not to embed.

rule 4 Don’t be afraid of application-level joins: if you index correctly and use the projection specifier (as shown in part 2) then application-level joins are barely more expensive than server-side joins in a relational database.

rule 5 Consider the write/read ratio when denormalizing. A field that will mostly be read and only seldom updated is a good candidate for denormalization: if you denormalize a field that is updated frequently then the extra work of finding and updating all the instances is likely to overwhelm the savings that you get from denormalizing.

rule 6 As always with MongoDB, how you model your data depends – entirely – on your particular application’s data access patterns. You want to structure your data to match the ways that your application queries and updates it.

reference [6 Rules of Thumb](https://www.mongodb.com/blog/post/6-rules-of-thumb-for-mongodb-schema-design-part-3)

  
  

Demonstrate, using your own code-samples, decisions you have made regarding → normalization vs denormalization 

Explain, using a relevant example, a full JavaScript backend including relevant test cases to test the REST-API (not on the production database)

Mini project, we are still working on this..
