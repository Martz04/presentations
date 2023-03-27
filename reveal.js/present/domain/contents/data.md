## The need for Quality Code


## Why do our designs go wrong?

Chaotic sofware systems are characterized by the same:

**Everything is coupled to everything else**

Changing any part of the of the system becomes hard, error prone and time consuming.


<!-- .slide: data-transition="fade" -->
![Big ball of Yarn](/present/domain/assets/yarn.png "Big ball of yarn")




## Encapsulation and Abstraction
The term encapsulation covers two closely related ideas:
_simplifying behavior_ and _hiding data_.

We **encapsulate behavior** by identifying a task that needs to be done in our code and giving that task to a well-defined object or function.

We call that object or function an *abstraction*.


<!-- .slide: data-transition="fade" -->
```python
import json
from urllib.request import urlopen
from urllib.parse import urlencode

params = dict(q='Sausages', format='json')
handle = urlopen('http://api.duckduckgo.com' + '?' + urlencode(params))
raw_text = handle.read().decode('utf8')
parsed = json.loads(raw_text)

results = parsed['RelatedTopics']
for r in results:
    if 'Text' in r:
        print(r['FirstURL'] + ' - ' + r['Text'])
```


<!-- .slide: data-transition="fade" -->
```python
import requests

params = dict(q='Sausages', format='json')
parsed = requests.get('http://api.duckduckgo.com/', params=params).json()

results = parsed['RelatedTopics']
for r in results:
    if 'Text' in r:
        print(r['FirstURL'] + ' - ' + r['Text'])
```


<!-- .slide: data-transition="fade" -->
Both code listings do the same thing:

They submit form-encoded values to a URL in order to use a search engine API.

But the second is simpler to read and understand _because it operates at a higher level of abstraction_.


<!-- .slide: data-transition="fade" -->
We can take this one step further still _by identifying and naming the task_ we want the code to perform for us and using an even higher-level abstraction to *make it explicit*:

```python
import duckduckgo
for r in duckduckgo.query('Sausages').results:
    print(r.url + ' - ' + r.text)
```


**Encapsulating behavior by using abstractions is a powerful tool for making code more expressive, more testable, and easier to maintain.**


<!-- .slide: data-transition="fade" -->
In a traditional OO language there is a form of _interface_, _abstract class_ to create these abstractions.
We can copy that behavior in Python, but remember Python can also use **Duck typing**


<!-- .slide: data-transition="fade" -->
`Encapsulation` and `Abstraction` help us by hiding details and protecting the consistency of our data.

We also need to pay attention to **the interactions** between our objects and functions.

**When one function, module, or object uses another, we say that the one depends on the other. These dependencies form a kind of network or graph.**



# Layering


<!-- .slide: data-transition="fade" -->
In a layered architecture, we divide our code into discrete categories or roles, and we introduce rules about which categories of code can call each other.
![Layering](/present/domain/assets/layering.png "Layer Architecture")



## SOLID Principles
**S.O.L.I.D.** is an acronym for the first five object-oriented design (OOD) principles by Robert C. Martin

These principles establish practices for maintaining and extending the codebase as the project grows.

Adopting these practices can also contribute to avoiding code smells, refactoring code, and Agile or Adaptive software development.


<!-- .slide: data-transition="fade" -->
- S - Single-responsiblity Principle.
- O - Open-closed Principle.
- L - Liskov Substitution Principle.
- I - Interface Segregation Principle.
- D - Dependency Inversion Principle.


<!-- .slide: data-transition="fade" -->
### Single-Responsibility Principle
Single-responsibility Principle (SRP) states:

A class should have one and only one reason to change, meaning that a class should have only one job.


<!-- .slide: data-transition="fade" -->
`User` class has two responsibilities: saving the user data to the database and sending an email to the user.
```python [7|13]
class User:
    def __init__(self, name, age, email):
        self.name = name
        self.age = age
        self.email = email

    def save_to_database(self):
        db = DBManager()
        db.connect()
        db.save(self)
        db.disconnect()

    def send_email(self, subject, message):
        sender = EmailSender()
        sender.connect()
        sender.send(self.email, subject, message)
        sender.disconnect()
```


<!-- .slide: data-transition="fade" -->
In the refactored code, we have separated the responsibilities of saving the user to the database and sending an email to the user into two separate classes.
```python [1 | 7 | 16]
class User:
    def __init__(self, name, age, email):
        self.name = name
        self.age = age
        self.email = email

class UserDatabaseSaver:
    def __init__(self):
        self.db = DBManager()

    def save_to_database(self, user):
        self.db.connect()
        self.db.save(user)
        self.db.disconnect()

class UserEmailSender:
    def __init__(self):
        self.sender = EmailSender()

    def send_email(self, user, subject, message):
        self.sender.connect()
        self.sender.send(user.email, subject, message)
        self.sender.disconnect()
```


<!-- .slide: data-transition="fade" -->
### Open-Closed Principle
Open-closed Principle (OCP) states:

Objects or entities should be open for extension but closed for modification.


<!-- .slide: data-transition="fade" -->
If we were to add a new shape, say a triangle, we would have to modify the total_area  method to include the area calculation for the new shape.
```python [1-7 | 9-16 | 18-24 | 30-37]
class Shape:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def area(self):
        pass

class Rectangle(Shape):
    def __init__(self, x, y, width, height):
        super().__init__(x, y)
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height

class Circle(Shape):
    def __init__(self, x, y, radius):
        super().__init__(x, y)
        self.radius = radius
    
    def area(self):
        return 3.14 * self.radius ** 2

class AreaCalculator:
    def __init__(self, shapes):
        self.shapes = shapes
    
    def total_area(self):
        total = 0
        for shape in self.shapes:
            if isinstance(shape, Rectangle):
                total += shape.width * shape.height
            elif isinstance(shape, Circle):
                total += 3.14 * shape.radius ** 2
        return total
```


<!-- .slide: data-transition="fade" -->
```python [12-13]
class AreaCalculator:
    def __init__(self, shapes):
        self.shapes = shapes
    
    def total_area(self):
        total = 0
        for shape in self.shapes:
            if isinstance(shape, Rectangle):
                total += shape.width * shape.height
            elif isinstance(shape, Circle):
                total += 3.14 * shape.radius ** 2
            elif isinstance(shape, Triangle):
                total += shape.width * shape.height / 2
        return total
```


To make this code comply with the OCP, we could introduce a new method `get_area`  in the Shape class and implement it in the subclasses. 

We could then remove the type checks from the `total_area`  method and simply call the `get_area` method on each shape.



```python [1,6 | 9,15-16 | 18,23-24 | 26,30-35]
class Shape:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def get_area(self):
        pass

class Rectangle(Shape):
    def __init__(self, x, y, width, height):
        super().__init__(x, y)
        self.width = width
        self.height = height
    
    def get_area(self):
        return self.width * self.height

class Circle(Shape):
    def __init__(self, x, y, radius):
        super().__init__(x, y)
        self.radius = radius
    
    def get_area(self):
        return 3.14 * self.radius ** 2

class AreaCalculator:
    def __init__(self, shapes):
        self.shapes = shapes
    
    def total_area(self):
        total = 0
        for shape in self.shapes:
            total += shape.get_area()
        return total
```


<!-- .slide: data-transition="fade" -->
### Liskov Substitution Principle
Liskov Substitution Principle (LSP) states:

Every subclass or derived class should be substitutable for their base or parent class.


The problem with this implementation is that a `Square` is not always a substitute for a `Rectangle`. 
If we were to create a `Square` object and pass it to a method that expects a `Rectangle`, the behavior of the program would be incorrect. 

For example, if we were to call the `set_height` method with a different value than the `width`, we would get incorrect results.
```python [1,3-4 | 12,15, 18-19 | 21,25-31]
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def get_width(self):
        return self.width
    
    def get_height(self):
        return self.height
    
    def set_width(self, width):
        self.width = width
    
    def set_height(self, height):
        self.height = height
    
    def area(self):
        return self.width * self.height

class Square(Rectangle):
    def __init__(self, side):
        super().__init__(side, side)
    
    def set_width(self, width):
        self.width = width
        self.height = width
    
    def set_height(self, height):
        self.width = height
        self.height = height
```


<!-- .slide: data-transition="fade" -->
we could refactor the `Square` class to avoid overriding the `set_width` and `set_height` methods. 
We could instead add a constructor that takes the side of the square and sets the width and height to the same value.
```python [21-23]
class Rectangle:
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def get_width(self):
        return self.width
    
    def get_height(self):
        return self.height
    
    def set_width(self, width):
        self.width = width
    
    def set_height(self, height):
        self.height = height
    
    def area(self):
        return self.width * self.height

class Square(Rectangle):
    def __init__(self, side):
        super().__init__(side, side)
```


<!-- .slide: data-transition="fade" -->
### Interface Segregation Principle
Interface segregation Principle (ISP) states:

A client should never be forced to implement an interface that it doesn’t use, or clients shouldn’t be forced to depend on methods they do not use.


<!-- .slide: data-transition="fade" -->
The problem with this implementation is that clients of `MultiFunctionPrinter` might not need to use all three methods.
```python [1,2,5,8 | 11,12,15,18]
class Machine:
    def print_document(self, document):
        raise NotImplementedError
    
    def scan_document(self):
        raise NotImplementedError
    
    def fax_document(self, document):
        raise NotImplementedError

class MultiFunctionPrinter(Machine):
    def print_document(self, document):
        print("Printing document...")
    
    def scan_document(self):
        print("Scanning document...")
    
    def fax_document(self, document):
        print("Faxing document...")
```


<!-- .slide: data-transition="fade" -->
we could refactor the `Machine` interface to include separate interfaces for printing, scanning, and faxing, as shown below:
```python [1,2 | 5,6 | 9,10 | 13]
class Printer:
    def print_document(self, document):
        raise NotImplementedError

class Scanner:
    def scan_document(self):
        raise NotImplementedError

class FaxMachine:
    def fax_document(self, document):
        raise NotImplementedError

class MultiFunctionPrinter(Printer, Scanner, FaxMachine):
    def print_document(self, document):
        print("Printing document...")
    
    def scan_document(self):
        print("Scanning document...")
    
    def fax_document(self, document):
        print("Faxing document...")
```


<!-- .slide: data-transition="fade" -->
### Dependency Inversion Principle
Dependency inversion Principle (DIP) states:

Entities must depend on abstractions, not on concretions. It states that the high-level module must not depend on the low-level module, but they should depend on abstractions.

This principle allows for decoupling.


The problem with this implementation is that the `ProductService` class is tightly coupled to the `MySQLDatabase` class.
If we were to change the database technology or switch to a different database vendor, we would need to modify the `ProductService` class.
```python [1,3,10,12 | 17-19]
class MySQLDatabase:
    def __init__(self, host, username, password, database):
        self.connection = MySQLdb.connect(
            host=host,
            user=username,
            passwd=password,
            db=database
        )
    
    def query(self, sql):
        cursor = self.connection.cursor()
        cursor.execute(sql)
        results = cursor.fetchall()
        cursor.close()
        return results

class ProductService:
    def __init__(self):
        self.db = MySQLDatabase("localhost", "root", "password", "mydatabase")
    
    def get_product(self, product_id):
        sql = "SELECT * FROM products WHERE id = {}".format(product_id)
        return self.db.query(sql)
```


<!-- .slide: data-transition="fade" -->
we could introduce an abstraction for the database connection and querying. 
For example, we could create an interface called Database that defines the methods for connecting to a database and executing a query. 

We could then modify the `MySQLDatabase` class to implement the `Database` interface.

```python [1-3 | 5,14 | 20 - 22]
class Database:
    def query(self, sql):
        raise NotImplementedError

class MySQLDatabase(Database):
    def __init__(self, host, username, password, database):
        self.connection = MySQLdb.connect(
            host=host,
            user=username,
            passwd=password,
            db=database
        )
    
    def query(self, sql):
        cursor = self.connection.cursor()
        cursor.execute(sql)
        results = cursor.fetchall()
        cursor.close()
        return results

class ProductService:
    def __init__(self, db):
        self.db = db
    
    def get_product(self, product_id):
        sql = "SELECT * FROM products WHERE id = {}".format(product_id)
        return self.db.query(sql)
```


<!-- .slide: data-transition="fade" -->
With this modification, the `ProductService` class no longer depends on the `MySQLDatabase` class directly. 
Instead, it depends on the `Database` interface, which can be implemented by any class that handles database connections and querying. 

This allows us to change the database technology or vendor without modifying the `ProductService` class.



# Summary

One of the most common reasons that our designs go wrong is that business logic becomes spread throughout the layers of our application, making it hard to identify, understand, and change.
