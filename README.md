# Passport-Local

[Passport](http://passportjs.org/) strategy for authenticating with a username
and password.

This module lets you authenticate using a username and password in your Node.js
applications.  By plugging into Passport, local authentication can be easily and
unobtrusively integrated into any application or framework that supports
[Connect](http://www.senchalabs.org/connect/)-style middleware, including
[Express](http://expressjs.com/).

## Installation

    $ npm install passport-local

## Usage

#### Configure Strategy

The local authentication strategy authenticates users using a username and
password.  The strategy requires a `verify` callback, which accepts these
credentials and calls `done` providing a user. 

    passport.use(new LocalStrategy(
      function(username, password, done) {
        User.findOne({ username: username }, function (err, user) {
          if (err) { return done(err); }
          if (!user) { return done(null, false); }
          if (!user.verifyPassword(password)) { return done(null, false); }
          return done(null, user);
        });
      }
    ));

The strategy expects to be called by a route handling a form, which by default 
must contain fields named `username` and `password`. You can use differently 
named fields by passing an options object:

    passport.use(new LocalStrategy(
      {
        usernameField: 'handle',
        passwordField: 'password'
      }
     ,function(username, password, done) {
       // verify credentials, then call done(err,user) or done(err,false)
      }
    ));

#### Authenticate Requests

Use `passport.authenticate()`, specifying the `'local'` strategy, to
authenticate requests.

For example, as route middleware in an [Express](http://expressjs.com/)
application:

    app.post('/login', 
      passport.authenticate('local', { failureRedirect: '/login' }),
      function(req, res) {
        res.redirect('/');
      });

#### Access request object
Setting the `passReqToCallback` option to `true` makes the Strategy's verification 
function receive the `req` object as the first parameter. 

Most of the time, just having the username and password will be adequate. 
Use this option if you are storing data in your request that your verification 
function needs - perhaps a user has already logged in with openid, and you want 
to link that user to a local user/pass.

    passport.use(new LocalStrategy(
      { passReqToCallback: true }
     ,function(req, username, password, done) {
        database.fetch(username, password, function(err, localUser){
          if(!err) {
            if(req.user.twitterID && (localUser.twitterID == null)) { 
              localUser.twitterID = req.user.twitterID;
              database.stash(localUser);
              return done(null, localUser);
            } else {
              return done(null, localUser);
            };
          } else {
            return done(err);
          };
        });
      }
    ));


## Examples

For a complete, working example, refer to the [login example](https://github.com/jaredhanson/passport-local/tree/master/examples/login).

## Tests

    $ npm install --dev
    $ make test

[![Build Status](https://secure.travis-ci.org/jaredhanson/passport-local.png)](http://travis-ci.org/jaredhanson/passport-local)

## Credits

  - [Jared Hanson](http://github.com/jaredhanson)

## License

(The MIT License)

Copyright (c) 2011 Jared Hanson

Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of
the Software, and to permit persons to whom the Software is furnished to do so,
subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS
FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR
COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER
IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN
CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
