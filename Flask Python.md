# Flask Python Library 
Flask is a micro web framework for Python. It is classified as a "microframework" because it provides a core set of features for web development, such as routing and request handling, but it does not include built-in components for things like database abstraction layers, form validation, or specific project layouts.

---

## ⬇️ How to install Flask
````bash
 pip install flask
````

## ❔ How to use Flask
````bash
from flask import Flask 

app = Flask(__name__)

@app.route('/')
def index():
    return 'Hello World'

if __name__ == '__main__':
    app.run()
````
