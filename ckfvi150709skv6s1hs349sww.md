## How to start Python Flask REST API

If you haven't start with Python flask lets start with me. Lets understand what is Flask ? 
![Flask Image](https://flask.palletsprojects.com/en/1.1.x/_images/flask-logo.png)

 [Flask](https://pypi.org/project/Flask/) is lightweight Python framework support as web interface. If you would like to create REST API or web interface Flask will you help to kick start as fast as possible. Like Python, Flask has larger communities and extra plugins available to enhance features. And its third party Python library. 

**Perquisites**:  Python 2.x or 3.x 

1. Installed Flask : `pip install -U Flask`
2. Create project folder "TestAPI" 
3. Create file call `app.py` this will works as application i am using this file name as root or base file of project.
4. Create simple code : 

```
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello():
    return "Welcome to Flask World!😃"
``` 
5. Run application 

```
$ env FLASK_APP=app.py flask run
 * Serving Flask app "app"
 * Running on http://127.0.0.1:5000/ (Press CTRL+C to quit)
``` 
Here is your first initial Flask running project. 

I have created one sample REST API project using Flask framework. 

%[https://github.com/aviboy2006/flask-rest-api]

This project can take as reference for project structure and how error handling can be done. What are naming convention can be used. 

Feel free to ask any question. Can reach out to my  [twitter](https://twitter.com/aviboy2006)  handler.


Reference : 
https://flask.palletsprojects.com/en/1.1.x/
https://opensource.com/article/18/4/flask
https://realpython.com/tutorials/flask/
