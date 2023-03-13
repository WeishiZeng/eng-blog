javascript
variable is lexical scope,
but the value is dynamically bind

https://www.geeksforgeeks.org/callable-future-java/
https://blog.sessionstack.com/how-javascript-works-event-loop-and-the-rise-of-async-programming-5-ways-to-better-coding-with-2f077c4438b5

https://www.promisejs.org/implementing/


start(done);


getServer();

=======

let serverInstance;


exports.start = function(cb) {
  db.connectDB(function() {
    //exports.serverInstance =
    serverInstance = http.createServer(app);
    serverInstance.listen(port, function() {
      console.log(TAG + "Listening to port: " + port);
       cb && cb();
    });
  });
}

exports.shutdown = function(cb) {
  console.log(TAG + "Shutting down service...");
  db.closeDB(function() {
    if (serverInstance) {
      serverInstance.close(cb);
    }
  });
}

exports.getServer = function() {
  return serverInstance;
};

vs.
exports.getServer = serverInstance;
