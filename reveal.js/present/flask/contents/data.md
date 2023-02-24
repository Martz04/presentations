## Building Web applications



## Client - Server Model
![Client Server Model](/present/flask/assets/client-server.png "client server model")


<!-- .slide: data-transition="fade" -->
Client- Server Model is the paradigm by which moderns systems are designed,
which consists of clients requesting data or service from servers and servers providing data or service to clients.

The browser does a DNS query to find out what is the IP address of the server.
Domain Name System describes entities and protocols involved in the translation from domain names to IP addresses.

The IP Address is the unique identifier that some entity granted to the server.


<!-- .slide: data-transition="fade" -->
A Server listen for requests on specific **ports**.
When communicating with another machine **you need to specify the port you want to communicate on**.

For example if a Client wants to speak to a server with HTTP it uses **port 80** if using HTTPS is uses **port 443**.



## OSI Model
The Open Systems Interconnection (OSI) model describes seven layers that computer systems use to communicate over a network. 

The modern Internet is not based on OSI, but on the simpler TCP/IP model. However, the OSI 7-layer model is still widely used, as it helps visualize and communicate how networks operate, and helps isolate and troubleshoot networking problems.


<!-- .slide: data-transition="fade" -->
![OSI Model](/present/flask/assets/osi-model.png)


<!-- .slide: data-transition="fade" -->
### IP
Internet Protocol, acronym used for IP Address.
Modern internet operates following the IP protocol.

The fundamental unit of data sent for 1 machine to another.
When you have multiple IP packets, you don't have a way to guarantee all packets will be received nor the order.


<!-- .slide: data-transition="fade" -->
### TCP
Transmission control protocol
Meant to sent IP packets in order in a reliable way and in an error free way.
Build on top of IP protocol

**When a machine communicates to another machine thru TCP is going to create a TCP connection between them**

Just for transportation of data. with HTTP we can also add business logic create server/client etc.


<!-- .slide: data-transition="fade" -->
### HTTP
Hypertext Transfer Protocol
Introduces a higher level abstraction on top of IP and TCP protocols

```txt
host: localhost
port: 8080
method: GET, POST, etc.
path: /payments logic for the server
headers: {
content-type metadata of the request.
}
body: {}
```


<!-- .slide: data-transition="fade" -->
### Architecture Characteristics
Are the three most important measures of the performance of a system

**Latency**
How long it takes a client receive data from server ?

**Availability**
Can we guarantee that the server is up 364/24? 

**Throughput**
How much processing can a server withstand?



## Flask Framework
**Flask** is a lightweight Python web framework that provides useful tools and features for creating web applications.

You can build a web application quickly using only a single Python file. 

Flask is also extensible and doesn’t force a particular directory structure or require complicated boilerplate code before getting started.


<!-- .slide: data-transition="fade" -->
## Installing Flask
To create a virtual environment on your system; run this command:
```shell
For mac/unix users: python3 -m venv env
For windows users: py -m venv env
```

After creating the environment, activate it by running :
```shell
For mac/unix users: source env/bin/activate
For windows users: .\env\Scripts\activate
```

You can deactivate it by simply running the command below, but you don't have to deactivate it yet.
```shell
deactivate
```


Install flask using pip
```shell
pip install flask
```


<!-- .slide: data-transition="fade" -->
## Requirements File
Using a Python requirements file comes with a lot of benefits:

1. Track of the Python modules and packages used by your project.

2. Easy to share your project with others.

3. To update or add a Python module to your project, you simply update the requirements file.


Generate requirements.txt from all installed Python modules
```shell
pip freeze > requirements.txt
```

You can specify versions
```text
uvicorn==0.12.2
fastapi==0.63.0
```

Installing Python pacakges from requirements file
```shell
pip install -r requirements.txt
```


<!-- .slide: data-transition="fade" -->
## Sample App
flsk_app/app.py
```python
from flask import Flask
app = Flask(__name__)

@app.route("/")
def hello():
  return "Hello World!"

if __name__ == "__main__":
  app.run(host="0.0.0.0", debug=True, port=5000)
```


<!-- .slide: data-transition="fade" -->
Create your Flask application instance, giving it the name `app`. 

You pass the special variable `__name__`, 
This name tells the instance where it’s located; you need this because Flask sets up some paths behind the scenes.
```python [1-2]
from flask import Flask
app = Flask(__name__)
```


<!-- .slide: data-transition="fade" -->
`@app.route` is a decorator that turns a regular Python function into a Flask view function, 
which converts the function’s return value into an HTTP response to be displayed by an HTTP client,
such as a web browser.
```python [1]
@app.route("/")
def hello():
  return "Hello World!"
```


<!-- .slide: data-transition="fade" -->
Runs Python script and creates a new Server with the address `0.0.0.0` on port `5000`
```python
if __name__ == "__main__":
  app.run(host="0.0.0.0", debug=True, port=5000)
```

    python app.js



## Request Parameters
The URL parameters are available in `request.args` dictionary.

```python [3-4]
@app.route('/my-route')
def my_route():
  page = request.args.get('page', default = 1, type = int)
  filter = request.args.get('filter', default = '*', type = str)
```

    curl "http://localhost:5000/my-route?page=10&filter=asd


<!-- .slide: data-transition="fade" -->
You can also use brackets `<>` on the URL of the view definition and this input will go into your view function arguments

```python [1|2]
@app.route('/<name>')
def my_view_func(name):
    return name
```
    curl "http://localhost:5000/mario


<!-- .slide: data-transition="fade" -->
For posted form input, use `request.form`

```python [1|3-4]
@app.route('/test', methods=[´POST´])
def my_view_func():
    email = request.form.get('email')
    password = request.form.get('password')
    return f"Hello {username}"
```

    curl -i -H "Content-Type: application/x-www-form-urlencoded" -X POST -d "email=mario@tec.com&password=123" http://localhost:5000/test


<!-- .slide: data-transition="fade" -->
For JSON posted with content type `application/json`, use `request.get_json()`.

```python [1|5|6]
from flask import request, jsonify

@app.route('/test', methods=[´POST´])
def my_view_func():
    data = request.get_json()
    return jsonify(data)
```

    curl -i -H "Content-Type: application/json" -X POST -d '{"userId":"1", "username": "fizz bizz"}' http://localhost:5000/test