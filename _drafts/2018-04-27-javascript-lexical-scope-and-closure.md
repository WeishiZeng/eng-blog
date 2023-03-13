# Callback and Lexical Scope in Javascript

## The "callback" style
For a simple task of querying database and print the result, let's consider the following two ways of programming.

In java, we usually wait for a blocking JDBC call and process the results in next line.
```java
ResultSet rs = stmt.executeQuery(query);

while (rs.next()) {
    System.out.println(rs.getInt("ID"));
}
```

In javascript, we usually pass a callback to the mongodb driver.
```javascript
let printAll = function (err, documents) {
  if (err) throw err;
  for (doc of documents) {
    console.log(doc);
  }
}

collection.find(query).toArray(printAll);
```


callback vs. thread
continuation passing style vs. direct style


Closure is when a function is able to remember and access its lexical scope even when that function is executing outside its lexical scope.
