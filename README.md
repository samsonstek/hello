# hello

This is a Hello web application made by using flask and Flask-SQLAlchemy.

# what does it do?

It creates a hello application that says hello and then says a name next to it. And that name is stored inside of a database and read from a database back to our web application.

# About Flask
Installation

To install Flask: 
```bash
pip3 install flask
```

To use it in python script
```bash
from flask import Flask
```

# About Flask-SQLAlchemy
Installation 

To install Flask-SQLAlchemy: 
```bash
pip3 install flask-sqlalchemy
```
To use it in python script
```bash
from flask_sqlalchemy import SQLAlchemy
```
## Steps followed in creating this web application

### 1. Create and Run a Hello world application in Flask
- Created a flask application by the name flask_hello_app.py
- imported the Flask class
```python
from flask import Flask
```
- instantiate our application
```python
app = Flask(__name__)
```
- Tell our Flask application which endpoint to listen to for connections, We want to listen to the homepage route
```python
@app.route(‘/’)
```
- Create a route handler named index and return a string that says ‘Hello World’
```python
def index():
     return ‘Hello World’
```
- Run Flask application which will launch a Development server on 127.0.0.1 and port 5000
```python
FLASK_RUN=flask_hello_app.py flask run
```
To enable Live reload(restart server and capture latest change), we can alternatively use the following and enable debug mode
```python
FLASK_RUN=flask_hello_app.py FLASK_DEBUG flask run
```

- When we open the URL on a browser, We get a ‘Hello World’ application running of a flash web server.

### 2. Incorporating Flask-SQLAlchemy and talk to a real database
- Import SQLAlchemy
```python
from flask_sqlalchemy import SQLAlchemy
```
- Link an instance of a database to interact with in SQLAlchemy to our Flask application
```python
db = SQLAlchemy(app)
```
db is an instance of the database
- Connect to our database from our Flask application by setting a configuration variable which is det on app.config
- Set a configuration variable( Flask-SQLAlchemy  expects a configuration variable ‘SQLALCHEMY_DATABASE_URI’) and equals to a string Flask-SQLAlchemy will understand to establish a connection
```python
app.config[‘SQLALCHEMY_DATABASE_URI’] = ‘the dialect://username:password@host:port//database_name’
```
In our case: 
```python
app.config[‘SQLALCHEMY_DATABASE_URI’] = ‘the dialect://postgresql:postgres@localhost:5432//example’
```
#### So We have created a ‘Hello World’ Application, Configured a connection from SQLAlchemy to our Flask Application

### 3. Create a Class and define attributes
```python
class Person(db.Model):
     __tablename__ = ‘persons’
     id = db.Column(db.Integer, primary_key = True)
     name = db.Column(db.String(), nullable = False)
```
#### This shows a model we use inside our web application

### 4 Create the tables in our database
- To detect the models, we use a method called db.create_all()
```python
db.create_all()
```
### 5 Insert a record to the created table and make to say the name instead of ‘Hello World’
- Import the created Person and db class on the terminal
```python
from flask_hello_app import Person, db
```
- create a variable
```python
person = Person(name = ‘john’)
```
- Add that variable
```python
db.session.add(person)
```
-Commit the added record to the database
```python
db.session.commit()
```
- Make it fetch the name
```python
def index():
     person = Person.query.first()
     return ‘Hello ‘ + person.name
``` 

### 6 Define a dander repr method on the class defined for debugging SQLAlchemy objects(optional) 
```python
def __repr__(self):
    return f’<Person ID: {self.id}, name: {self.name}>’
# hello
