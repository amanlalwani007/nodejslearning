# Node js Learning


Node js is a javascript runtime which enables you to execute javascript outside  of browser.

### **Creating a project**
```
1. npm init. 
2. npm install package_name (install the package locally).
3. npm install -g package_name (install the  package globally).
4. npm install -D package_name (if you want to install module for dev only).
5. npm  install  (if you  deleted node modules folder and  want to install dependencies  then npm install witll automatically pick dependencies from package.json and install it).
6. To open shell to run  javascript ,run command node in terminal.
```

### **Run a javascript file** 

```js 
node index.js 
Output:-
Hello  world 
```
### **To Export a object outside module**
```js
module.exports=person
``` 
### **To import a exported object**

```js 
const person= require('./person')
console.log(person)
```

### **Make a class and export it and use it in index.js**
```js
//Person.js

class Person{
    constructor(name,age){
        this.name=name;
        this.age=age
    }

    greeting(){
        console.log(`My name is ${this.name} and age is ${this.age}`)
    }
}

module.exports=Person

//index.js

const Person= require('./person')
const person1 = new  Person("aman",20)
person1.greeting()

//Output

$node index.js 
My name is aman and age is 20
```
### **Working With path Package**

```js
const path = require('path');

// Base file name
console.log(path.basename(__filename))

//Directory path name 

console.log(path.dirname(__filename))

// file extension
console.log(path.extname(__filename))

//create path project 
console.log(path.parse(__filename))

//get only base fle name

console.log(path.parse(__filename).base)

// concatenate paths 
console.log(path.join(__dirname,'test','hello.html'))



Output:-
 path_demo.js
/home/amanlalwani/Desktop/Desktop/node_crash_course/reference
.js
{
  root: '/',
  dir: '/home/amanlalwani/Desktop/Desktop/node_crash_course/reference',
  base: 'path_demo.js',
  ext: '.js',
  name: 'path_demo'
}
path_demo.js
/home/amanlalwani/Desktop/Desktop/node_crash_course/reference/test/hello.html
```



### **Working With fs Package**

```js
const fs = require("fs");
const path = require("path");

//create folder
fs.mkdir(path.join(__dirname, "/test"), {}, (err) => {
  if (err) throw err;
  console.log("folder created ");
});

//create and write  to  file and append to it 

fs.writeFile(
  path.join(__dirname, "/test", "hello.txt"),
  "Hello World",
  (err) => {
    if (err) throw err;
    console.log("file written to ....");
    fs.appendFile(
      path.join(__dirname, "/test", "hello.txt"),
      "I love node js ",
      (err) => {
        if (err) throw err;
        console.log("file written to ....");
      }
    );
  }
);

//Read from file
fs.readFile(path.join(__dirname, "/test", "hello.txt"), "utf8", (err, data) => {
  if (err) throw err;
  console.log(data);
});

//Rename a file
fs.rename(
  path.join(__dirname, "/test", "hello.txt"),
  path.join(__dirname, "/test", "helloworld.txt"),
  (err) => {
    if (err) throw err;
    console.log("File renamed");
  }
);


Output:-
folder created 
file written to ....
file written to ....
Hello WorldI love node js 
File renamed
```

### **Working With os Module**

```js
const os = require("os");

//platform
console.log(os.platform());

//cpu Arch
console.log(os.arch());

//CPU  Core  info
console.log(os.cpus());

//free memory
console.log(os.freemem());

//total memory
console.log(os.totalmem());

//home dir
console.log(os.homedir());

//Uptime
console.log(os.uptime());

Output:- 
linux
x64
[
  {
    model: '11th Gen Intel(R) Core(TM) i5-1135G7 @ 2.40GHz',
    speed: 1577,
    times: {
      user: 36448830,
      nice: 188950,
      sys: 43303780,
      idle: 306086880,
      irq: 0
    }
  }]
267997184
16157941760
/home/amanlalwani
550873.83
```

### **Working With url Module**

```js 
const url = require("url");

const myUrl = new URL(
  "http://mywebsite.com:8000/hello.html?id=10&status=active"
);

//Serialized  url

console.log(myUrl.href);
console.log(myUrl.toString());

//Host (root domain)
console.log(myUrl.host);

//hostname(does not contain ports)
console.log(myUrl.hostname);

//Pathname
console.log(myUrl.pathname);

//Serialized query
console.log(myUrl.search);
//Params object
console.log(myUrl.searchParams);

//Add param
myUrl.searchParams.append("abc", "123");
console.log(myUrl.searchParams);

//loop through params
myUrl.searchParams.forEach((value, name) => console.log(`${name}: ${value}`));

Output :-
http://mywebsite.com:8000/hello.html?id=10&status=active
http://mywebsite.com:8000/hello.html?id=10&status=active
mywebsite.com:8000
mywebsite.com
/hello.html
?id=10&status=active
URLSearchParams { 'id' => '10', 'status' => 'active' }
URLSearchParams { 'id' => '10', 'status' => 'active', 'abc' => '123' }
id: 10
status: active
abc: 123
```

### **Working With event Module**
```js 
const EventEmitter  =require('events')

//Create class
class MyEmitter extends EventEmitter {}

//Init object
const myEmitter=new MyEmitter()

//Event  listener
myEmitter.on('event',()=>console.log('Event  fired'));

//init event

myEmitter.emit('event')
myEmitter.emit('event')
myEmitter.emit('event')

Output:-
Event  fired
Event  fired
Event  fired

```
#### _Working example of event Module_

```js
//logger.js

const EventEmitter = require("events");
const uuid = require("uuid");

class Logger extends EventEmitter {
  log(msg) {
    // Call event
    this.emit("message", { id: uuid.v4(), msg });
  }
}
module.exports=Logger

//index,js

const Logger = require("./logger");
const logger = new Logger();

logger.on("message", (data) => console.log("Called Listener", data));

logger.log("Hello World");
logger.log("Hello");

Output :-
$node  index.js 
Called Listener { id: 'e9cde714-c54d-49e2-892f-132c5c6060e0', msg: 'Hello World' }
Called Listener { id: '82de770e-af59-46bd-bf91-b117d5c9efcf', msg: 'Hello' }
```
### **Working With http Module**

```js
const http = require("http");

//create server object
http
  .createServer((req, res) => {
    res.write("Hello world");
    res.end();
  })
  .listen(5000, () => console.log("Server running..."));

```
### **Sample Nodejs server that serves json and  html files**

```js
const http = require("http");
const path = require("path");
const fs = require("fs");

const server = http.createServer((req, res) => {
  if (req.url === "/") {
    fs.readFile(
      path.join(__dirname, "public", "index.html"),
      (err, content) => {
        if (err) throw err;
        res.writeHead(200, { "Content-Type": "text/html" });
        res.end(content);
      }
    );
  }
  if (req.url === "/about") {
    fs.readFile(
      path.join(__dirname, "public", "about.html"),
      (err, content) => {
        if (err) throw err;
        res.writeHead(200, { "Content-Type": "text/html" });
        res.end(content);
      }
    );
  }
  if (req.url === "/api/users") {
    const users = [
      { name: "aman lalwani", age: 23 },
      { name: "prem lalwani", age: 19 },
    ];
    res.writeHead(200, { "Content-Type": "text/html" });
    res.end(JSON.stringify(users));
  }
});

const PORT = process.env.PORT || 5000;

server.listen(PORT, () => console.log(`Server running on port ${PORT}`));
```

### **Dynamic Nodejs server that serves html files**

```js
const http = require("http");
const path = require("path");
const fs = require("fs");

const server = http.createServer((req, res) => {
  //Build file path
  let filePath = path.join(
    __dirname,
    "public",
    req.url == "/" ? "index.html" : req.url
  );

  //Extension of filename
  let extname = path.extname(filePath);

  //initial content  type
  let contentType = "text/html";

  //check ext and set content type
  switch (extname) {
    case ".js":
      contentType = "text/javascript";
      break;
    case ".css":
      contentType = "text/css";
      break;
    case ".json":
      contentType = "application/json";
      break;
    case ".png":
      contentType = "image/png";
      break;
    case ".jpg":
      contentType = "image/jpg";
      break;
  }
  // Read File
  fs.readFile(filePath, (err, content) => {
    if (err) {
      if (err.code === "ENOENT") {
        //page not found
        fs.readFile(
          path.join(__dirname, "public", "404.html"), (err, content) => {
            res.writeHead(200, { "Content-Type": "text/html" });
            res.end(content, "utf-8");
          });
      } else {
        res.writeHead(500);
        res.end(`Server Error: ${err.code}`);
      }
    } else {
      //Success
      res.writeHead(200, { "Content-Type": contentType });
      res.end(content);
    }
  });
});

const PORT = process.env.PORT || 5000;

server.listen(PORT, () => console.log(`Server running on port ${PORT}`));

```

### **Deployment Steps for heroku**
```
1. Install Heroku cli.
2. bash$ heroku login
3. add commit all the data inside repo except node_modules(add it into gitignore).
4. bash$ heroku create 
5. copy the deploy and push command from heroku ui and execute it sequentially.
6. bash$ heroku open (it will open the public url of our website after deployment) 
```
