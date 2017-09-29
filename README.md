# Angular and Heroku Deployment

>Modern full-stack deployment can be a little complicated.  We want the flexibility to be able to dynamically test our code locally, but we also want a quick-to-load, seamless app for our users.  In order to set this up, we need to do a few things.

## Before We Start

### Our Folder Structure

We will be following the patter of our [tunr](https://github.com/den-materials/modeling-tunr) [labs](https://github.com/den-materials/tunr-relationships) for this deployment.  That is, we will have a separate folder for the back end API, and one for the front end Angular code.

### Heroku Setup

If you haven't already, please read through [this guide](https://github.com/SF-WDI-LABS/shared_modules/blob/master/how-to/heroku-mean-stack-deploy.md) on deploying to Heroku.  

## Creating our Repo

1. Create a folder with the name of your project.
2. Go into the folder and initialize it as an npm repository with `npm init -y`.
3. Create a `front-end` folder with `ng new` if you don't already have one.
4. Create a `back-end` folder, go inside, initialize it as an npm repository, and create a `server.js` file.

## `server.js`

Put the following code inside your `server.js` file:

1. ```js
  require('dotenv').config();
  ```
  - This allows us to use `process.env` locally.
2. ```js
  const express = require('express');
  const app = express();
  const path = require('path');
  ```
  - This sets up our critical node packages.
3. ```js
  //CORS setup to allow other ports from this host

  //Only needed if not on Heroku/prod
  if(!process.env.DYNO) {
    app.use(function(req, res, next) {
      res.header("Access-Control-Allow-Origin", "http://localhost:4200");
      res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
      res.header("Access-Control-Allow-Methods", "POST, GET, OPTIONS, PUT, DELETE");
      next();
    });
  }
  ```
- This enables CORS so that your Angular front end can communicate with your back end without errors when coding locally.
4. ```js
  app.use(express.static(__dirname + '/dist'));

  app.get('/*', function(req, res) {
    res.sendFile(path.join(__dirname + '/dist/index.html'));
  });
  ```
  - The first line serves up the front end.  Make sure you put the `app.get` part below any back end routes, because it creates a route that defaults to the front end if no back end routes exist (by serving up the Angular `index.html` file).
5. ```js
  let port = process.env.PORT || 3000;

  app.listen(port, function() {
    console.log(`Listening on port ${port}`);
  });
  ```
  - This makes sure we can serve the app locally (3000) and on Heroku (process.env.PORT).

## Setting up the DB

### PSQL

### MongoDB
