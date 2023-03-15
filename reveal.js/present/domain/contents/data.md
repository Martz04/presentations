## The need for Quality Code


## Why do our designs go wrong?

Chaotic sofware systems are characterized by the same:

**Everything is coupled to everything else**

Changing any part of the of the system becomes hard, error prone and time consuming.


<!-- .slide: data-transition="fade" -->
![Big ball of Yarn](/present/domain/assets/yarn.png "Big ball of yarn")




# Encapsulation and Abstraction
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


### Single-Responsibility Principle
Single-responsibility Principle (SRP) states:

A class should have one and only one reason to change, meaning that a class should have only one job.


### Open-Closed Principle
Open-closed Principle (OCP) states:

Objects or entities should be open for extension but closed for modification.


### Liskov Substitution Principle
Liskov Substitution Principle (LSP) states:

Every subclass or derived class should be substitutable for their base or parent class.


### Interface Segregation Principle
Interface segregation Principle (ISP) states:

A client should never be forced to implement an interface that it doesn’t use, or clients shouldn’t be forced to depend on methods they do not use.


### Dependency Inversion Principle
Dependency inversion Principle (DIP) states:

Entities must depend on abstractions, not on concretions. It states that the high-level module must not depend on the low-level module, but they should depend on abstractions.

This principle allows for decoupling.



# Summary

One of the most common reasons that our designs go wrong is that business logic becomes spread throughout the layers of our application, making it hard to identify, understand, and change.
