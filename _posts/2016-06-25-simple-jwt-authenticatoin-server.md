---
layout: post
title: Our Simple JWT Authentication Server
comments: true
---
##Introduction to Express and JWT

If you're new to the `NodeJs` world then maybe you don't know much about [Express](http://expressjs.com/). But if you're not new at all then you know how popular `Express` is. Express is a really simple web framework that makes writing RESTful API's a cinch. Express will be serving our authentication server that we will use in later tutorials for our SPA build in React or Angular, and potentially for our mobile applications.

With our ambition to provide a RESTful service to different front-end applications -- we need to make our authentication process stateless. While normally it would be easy to provide sessions and cookies, the two inherently affect the state. With [JWT](https://jwt.io/) we issue a token that is signed by us and only verified by us. Since the token is passed on all requests and only verified against our `Secret` we can insure we remain stateless.

I recommend reading the [intro](https://jwt.io/introduction/) at the official `JWT` site. This will be a great primer for when you're unsure about what is going on.

> A BIG NOTE: This is not production ready at-all. A few reasons: Our Tokens aren't complete in hiding extraneous info. There is also managing token-sign-outs and password changes. While we have expirations it is possible to use another server to log logged-out tokens until they expire. Another thing is we do not have SSL to protect users from token capture. I do recommend taking the opportunity to check out [Lets Encrypt](https://letsencrypt.org/) if you're looking to secure your domain. Also you should be using your Env variable -- where here we just define them ourselves.

You can get ahold of the last commit [here](https://github.com/FullPint/simpleJwtAuthServer).  

### Getting Our Express Server Up And Running
First We start by running `npm init - y`. This will start our project. It will set all default options. We will be writing with some ES6 syntax when possible so we need to add [Babel.](#) I also like using absolute paths instead of relative ones so we will add `babel-root-import.` So run: `npm install --save morgan express` `npm install --save-dev babel-core babel-eslint babel-preset-es2015 babel-preset-stage-2 babel-root-import`.

You `package.json` should look like below:
{% gist db0554e69c958d5711bbc1d96ad4e9bc/9073514f146f13d08924484978e2e9aaace39f4a %}

Next create your `.babelrc` file in your root directory. We need to add our presets there.
Your `.babelrc` should look like this:
{% gist f544e857814d95b06f0140cd3130331b/1d99e92516c30d8dbac04e6001b9a96c4651bef3 %}

Since we don't really won't have to restart our project every-time we update we will globally install [nodemon.](#) -- `npm install -g nodemon`.

We need to update our start script inside `package.json` to take advantage of this. So next add `nodemon server.js --exec babel-node --presets es2015,stage-2` as out `start` script in `package.json`.
{% gist db0554e69c958d5711bbc1d96ad4e9bc/05080bc4d41bcb17e1460397f374448b30d3a722 %}

While all of this setup is good, we don't have our server setup going yet. Make a file in your root directory called `server.js`. This is will be the entry point for our authentication project.

Inside `server.js` we need to bring in our different modules to start the server and set our options. First following along the clone and I will explain after what's going on.

{% gist 4053ebdc1a2d46936b1ea86ac1155213/b30c5f8db402d2c0d5dd2b6bac829223f96333cb %}

We import `express` and set initialize it as the `const app`. The import `morgan`, set our `port` where the app is connected to. Next `morgan` is set to `dev` mode so we can see when errors are made. Las but not least we call `app.listen()`. This allows `express` to listen for connections, and we log a message.

Run `npm start` and you should see.
{% highlight bash %}
authServer@1.0.0 start /Users/user/authServer
nodemon server.js --exec babel-node --presets es2015,stage-2

[nodemon] 1.8.1
[nodemon] to restart at any time, enter `rs`
[nodemon] watching: *.*
[nodemon] starting `babel-node server.js --presets es2015,stage-2`
Your app is running on PORT: 1600
{% endhighlight %}

### Adding and Connecting to Mongoose

In order for us to be able to add and validate users we need a database. If you don't already have [mongodb](https://www.mongodb.com/) installed you will need to install it. You can find the docs on how to install mongo [here.](https://docs.mongodb.com/manual/tutorial/install-mongodb-on-os-x/)

We will be using a driver for `mongo` called [mongoose.](http://mongoosejs.com/) It offers an easy way for us to model our data, along with other built in modules that'll make our life a lot easier. To install `mongoose` run `npm install --save mongoose.` We will also setup a config file, create `server.config.js` in your root. In the `server.config.js` we will move our port and our database address inside of it. This is convenient for your `.gitignore` to hide this info in your repositories.

{% gist db0554e69c958d5711bbc1d96ad4e9bc/4d5337849bad52cc1f54f5d7edaf6b66d259654b %}
{% gist d5aac1f8b2c34ecad52d03ba2bf2268a/0b6c1a35d7350df2be9af738b496fab3bf8e0fb2 %}


### Creating the User Model

For us to be able to authenticate we need to create users to authenticate against. We do not need much from our user -- just a name and a password since this article is supposed to be a simple one. When we store our users' passwords we will not be storing them in plain text, but using a hashing algorithm called [bcrypt.](https://github.com/ncb000gt/node.bcrypt.js/)

I recommend reading [this article](http://blog.mongodb.org/post/32866457221/password-authentication-with-mongoose-part-1
) from the offical `mongo` site. It goes over how to authenticate using methods within the user model itself and our code is based on it.

We will create two new directories where this will live. A `models` directory and within that a `users` directory. And inside the users directory make a file called `user.model.js`.

{% highlight bash %}
.
├── app
│   ├── models
│       └── users
│           └── user.model.js
├── package.json
├── server.config.js
└── server.js

{% endhighlight %}

Now we need to run the command `npm install --save bcrypt` so that we will be able to use `bcrypt` to hash our use passwords.

{% gist db0554e69c958d5711bbc1d96ad4e9bc/05080bc4d41bcb17e1460397f374448b30d3a722 %}

Copy the following into your `user.model.js`

{% gist 3c51dee3d48ad0ca8d514bf9ef30f3a9/1ab81163b15bc8cef82b1ec07ea730cee97db03d %}

In `user.model.js` we defined our `UserSchema`, and exported it as `User` for use in our controllers later on. We also added some nifty methods within the model. The `.pre` with the paramter `save`, allows us to hash our password when modified before every save. And aslo added our own method called `comparePassword.` We will use this later when we attempt to authenticate a user when they sign in.

### Building the Router.

We need a router to process our `HTTP` requests. This will allow us to build an API to interact with. This article won't go into detail about building a full `API`, and we are just attempting to authenticate a user and pass some sort of confirmation.

Create a `routes/` directory in our `app/` directory. Then place a new file called `main.router.js`. This will be the top of our router allowing one main entry point.

{% highlight bash %}
.
├── app
│   ├── models
│   │   └── users
│   │       └── user.model.js
│   ├── routes
│       └── main.router.js
├── package.json
├── server.config.js
└── server.js

{% endhighlight %}

Next thing to do is to fill in `main.router.js`. We need to initialize the built in router from `express`. Then have an index route that responds with some sort of `JSON`

{% gist a5c1f4f994f7b963599125e92f3e0f53/e4c53f20409f929a210d3100d862ed5b786fb50e %}

With that done, our `server.js` needs to be able to use it. So we will import `mainRouter` and tell our `app` to `use` the module.

{% gist 4053ebdc1a2d46936b1ea86ac1155213/c45c22d21d309d3a621f6ee0c299184327e1f460 %}

### Using Post Man

We need to be able to test if our routes do as we say they will. [Postman](https://www.getpostman.com/) offers a way for us to make requests to our API.

Fire up Postman and make a GET request to http://localhost:1600/ and it should look like this.

![](http://alecdavila.me/public/images/2016-06-20-using-webpack/post-man-first-shot.png "postman screen shot")

### Building the Signup Process.

There now needs to be able to make a new user. So we are going to write `user.create.controller.js`. This will be placed inside of a new directory `controllers/users/`. The new structure should look like this.

{% highlight bash %}
.
├── app
│   ├── controllers
│   │   └── users
│   │       └── user.create.controller.js
│   ├── models
│   │   └── users
│   │       └── user.model.js
│   ├── routes
│   │   └── main.router.js
├── package.json
├── server.config.js
└── server.js
{% endhighlight %}

Inside of `user.create.controller.js` we bring in the `User` from our model and process our `(req, res)`. `Req` will carry the `body` of request from the client.

We first check and see if the `body` contains userName and password. Then create a newUser object and test it against our `save` method that we wrote for our model.

{% gist d667cdb52d267acb4ce3fed4713f4932/1aa7b93932b50005de650791a55e02b55d8470d1 %}

In order to parse the request we need to `npm install --save body-parser`.

The new `package.json`:

{% gist db0554e69c958d5711bbc1d96ad4e9bc/9df30a90fb5af6f7b47cb0d8dd8137ba4f4541ac %}

Now we need to update the `server.js` to use `body-parser`. And we set these two things

{% highlight javascript %}
app.use(boodyParser.urlencoded({
  extended: false
}))
app.use(boodyParser.json())
{% endhighlight %}

Using `urlencoded` and `extended` false option ensures we only accept `UTF-8` encoding and only accept key value pairs of arrays and strings. The we tell boodyParser to only accept JSON.

Now we need to modify `main.router.js`, to allow us to use the controller. We import the module and tell the router when a POST call is made to `/signup` that our `user.create.controller` handles it.

{% gist a5c1f4f994f7b963599125e92f3e0f53/66e2f61af93cdbb85c7ae33b04eb5d684129bbb5 %}

Now in postman send a POST call to http://localhost:1600/signup. You need to send both a `userName` and `password` as keys. And it should work!

### Loging In Users and Authorizing Requests

Before we write our controllers we neet add a package to create and decode JWT's. We will be using [jsonwebtoken.](https://github.com/auth0/node-jsonwebtoken) Run `npm install --save jsonwebtoken`.

We need two new controllers in `controllers/user` and they will be `user.login.controller` and `user.auth.controller`.

{% highlight bash %}
.
├── app
│   ├── controllers
│   │   └── users
│   │       ├── user.auth.controller.js
│   │       ├── user.create.controller.js
│   │       └── user.login.controller.js
│   ├── models
│   │   └── users
│   │       └── user.model.js
│   ├── routes
│        └── main.router.js
├── package.json
├── server.config.js
└── server.js
{% endhighlight %}


We will first be working with `user.login.controller`. It needs to be able to verify the userName and password -- create and sign a token -- the send it to the client.

{% gist 6f6da27ef97cabd3367470d453a2a261/896e05799996f779de648808ca132bb5adbfd2b3 %}

We then need to provide a route for it. So back in `main.router` we add this component and have it execute when the client asks for `/login`.

{% gist a5c1f4f994f7b963599125e92f3e0f53/09450c3dbeec4107bd3e7d245dec596e9f796229 %}

Now when you use postman and make a POST call at http://localhost:1600/login and pass the userName and password you signed up with earlier, you should receive a token as a response from the server. We will be using the token to authenticate requests moving forward.

Now our `user.auth.controller` will act as  [middleware](http://expressjs.com/en/guide/using-middleware.html) for requests. For this example we will protect our API using the middleware. A key part of middleware is the next statement so keep an eye out for it.

The module will take our request, either respond with error like "invalid token" or "No token provided" -- or allow us through to our actual request.

{% gist 8815c11235527f9b59a405ebd05b103b/ccef356ca802db8548d3ddc6b48ecf71994d8831 %}

Then hook it up to our router as before. This time though -- we will specify to use the module for all routes that fall underneath http://localhost:1600/api. We will aloso put in a route under it http://localhost:1600/api/v1 to respond with a message to show that it works.

{% gist a5c1f4f994f7b963599125e92f3e0f53/e0f7e8e35128dc8a25e16fe83ed5241f7b0951a5 %}

Now when you fire up postman and pass in the token with your request to http://localhost/api/v1 you should get a 'success response'.

## One Last Thing and Conclusion

We need to run `npm install --save cors` to add [cors.](https://github.com/expressjs/cors) This will allow us in later tutorials to use this auth server for our front-end applications.

With `express` and `JWT` we showed an easy way to both authenticate users and protect our API's from unwanted clients. This is really powerful as we can provide user specific experiences to  API's, Web Applications, and Mobile Applications.
