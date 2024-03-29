﻿# Skeleton Python
[![Build Status](https://travis-ci.com/derymukti/meeberpy.svg?branch=master)](https://travis-ci.com/derymukti/meeberpy)
[![codecov](https://codecov.io/gh/derymukti/meeberpy/branch/master/graph/badge.svg)](https://codecov.io/gh/derymukti/meeberpy)

Skeleton Python is skeleton python with flask.

  - PostgreSQL
  - Python 2.7
  - ORM (Sqlalchemy)
  - pip
  - Gunicorn
 
### Installation

Skeleton Python support [Python2.7](https://www.python.org/downloads/release/python-2716/) and [Python3.7](https://www.python.org/downloads/release/python-373/).

Config and run.
for configuration you can edit in file "config.json"

```sh
$ cd python-skeleton
$ pip install -r requirements.txt
$ python main.py migrate
$ python main.py seed
```

### Run program

#### Normal
```sh
$ python main.py run
```

#### With Gunicorn Server
```sh
$ python main.py start_gunicorn
```

gunicorn can run with __worker__ and __multithreading__, you can setup in `config.json`
and gunicorn can save logs file

### How To Create Controller?

Meeber python is simple architecture folder.

```sh
$ python main.py create:controller -n testing
```
##### this will be creating new endpoint "hostname:port/testing"

after run this command you will found 'testing' folder in controllers.
and new line code in file controllers/`__init__.py`
```python

from testing.router import testing
app.register_blueprint(testing)

```
example for testing controller:

| File | Description |
| ------ | ------ |
| controllers/`__init__.py` | this a public routing, u can register blueprint here |
| controllers/testing/`__init__.py` | u can import local router here |
| controllers/testing/`router.py` | this a local router , u can add more entities here |
| controllers/testing/modules/`__init__.py` | this a modules for use in your endpoint, u can add more function in here |

# How to create Entities?
1. open file controllers/yourcontroller/modules/`__init__.py` and add this to end of file
    ```python
    def get():
        return "hello?"
    ```
2. open file controllers/yourcontroller/router.py and add this to end of file
    ```python
    @yourcontroller.route('/new_entities',methods=['GET'])
    def get_data():
        return get()
    ```
    this will be creating new entities like "hostname:port/yourcontroller/new_entities" with GET method  
 - __get()__ is import from modules/`__init__.py`  
# How to return in json type?
1. cek your `__init__.py` file in `modules` make sure this import 
    ```python
    from manager.json_response import response
    ```
2. add this in function to return. ex:
    ```python
    def get():
        json_data={"title":"this title","description":"this description"}
        return response(data=json_data,code=200,messages="success")
    ```
    
    this will be return
    ```json
    {
        "meta": {
            "message": "success",
            "code": 200,
            "server": "cc15b6a3-fe2a-3294-8fec-6c5aee285a48"
        },
        "data": {
            "title":"this title",
            "description":"this description"
            }
    }
    ```
    | Name | Type | Description |
    | ------| ------ | ------ |
    | code | INT |this is code for header response and status |
    | message | String | message for response |
    | data | String, Array, Object | data for return |
