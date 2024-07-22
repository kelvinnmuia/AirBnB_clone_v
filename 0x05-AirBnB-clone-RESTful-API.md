# 0x05. AirBnB clone - RESTful API
## The Domains/Concepts covered in this project: `Python` `Back-end` `API` `Webserver` `Flask`

# More Info
![alt text](./aribnb_diagram_0.jpg)

## Install Flask

```
$ pip3 install Flask
```

## Tasks :page_with_curl:

**0. Restart from scratch!**

No no no! We are already too far in the project to restart everything.

But once again, let’s work on a new codebase.

For this project you will fork this [codebase](https://github.com/alexaorrico/AirBnB_clone_v2):

  * Update the repository name to AirBnB_clone_v3
  * Update the `README.md`:
    * Add yourself as an author of the project
    * Add new information about your new contribution
    * Make it better!
  * If you’re the owner of this codebase, create a new repository called `AirBnB_clone_v3` and copy over all files from `AirBnB_clone_v2`

  * [AirBnB_clone_v3](https://github.com/kelvinnmuia/AirBnB_clone_v3)

**1. Never fail!**

Since the beginning we’ve been using the `unittest` module, but do you know why `unittests` are so important? Because 
when you add a new feature, you refactor a piece of code, etc… you want to be sure you didn’t break anything.

At Holberton, we have a lot of tests, and they all pass! Just for the Intranet itself, there are:

  * `5,213` assertions (as of 08/20/2018)
  * `13,061` assertions (as of 01/25/2021)

The following requirements **must** be met for your project:
  * all current tests must pass (don’t delete them…)
  * add new tests as much as you can (tests are mandatory for some tasks)

```
guillaume@ubuntu:~/AirBnB_v3$ python3 -m unittest discover tests 2>&1 | tail -1
OK
guillaume@ubuntu:~/AirBnB_v3$ HBNB_ENV=test HBNB_MYSQL_USER=hbnb_test HBNB_MYSQL_PWD=hbnb_test_pwd HBNB_MYSQL_HOST=localhost HBNB_MYSQL_DB=hbnb_test_db HBNB_TYPE_STORAGE=db python3 -m unittest discover tests 2>&1 /dev/null | tail -n 1
OK
guillaume@ubuntu:~/AirBnB_v3$ 
```

  * [AirBnB_clone_v3](https://github.com/kelvinnmuia/AirBnB_clone_v3)

**2. Improve storage**

Update DBStorage and FileStorage, adding two new methods. All changes shoud be done in the branch `storage_get_count`:

A method to retrieve one object:

  * Prototype: def get(self, cls, id):
    * `cls`: class
    * `id`: string representing the object ID
  * Returns the object based on the class and its ID, or `None` if not found

  A method to count the number of objects in storage:

  * Prototype: `def count(self, cls=None):`
    * `cls`: class (optional)
  * Returns the number of objects in storage matching the given class. If no class is passed, returns the count of all objects in storage.

Don’t forget to add new tests for these 2 methods on each storage engine.

```
guillaume@ubuntu:~/AirBnB_v3$ cat test_get_count.py
#!/usr/bin/python3
""" Test .get() and .count() methods
"""
from models import storage
from models.state import State

print("All objects: {}".format(storage.count()))
print("State objects: {}".format(storage.count(State)))

first_state_id = list(storage.all(State).values())[0].id
print("First state: {}".format(storage.get(State, first_state_id)))

guillaume@ubuntu:~/AirBnB_v3$
guillaume@ubuntu:~/AirBnB_v3$ HBNB_MYSQL_USER=hbnb_dev HBNB_MYSQL_PWD=hbnb_dev_pwd HBNB_MYSQL_HOST=localhost HBNB_MYSQL_DB=hbnb_dev_db HBNB_TYPE_STORAGE=db ./test_get_count.py 
All objects: 1013
State objects: 27
First state: [State] (f8d21261-3e79-4f5c-829a-99d7452cd73c) {'name': 'Colorado', 'updated_at': datetime.datetime(2017, 3, 25, 2, 17, 6), 'created_at': datetime.datetime(2017, 3, 25, 2, 17, 6), '_sa_instance_state': <sqlalchemy.orm.state.InstanceState object at 0x7fc0103a8e80>, 'id': 'f8d21261-3e79-4f5c-829a-99d7452cd73c'}
guillaume@ubuntu:~/AirBnB_v3$
guillaume@ubuntu:~/AirBnB_v3$ ./test_get_count.py 
All objects: 19
State objects: 5
First state: [State] (af14c85b-172f-4474-8a30-d4ec21f9795e) {'updated_at': datetime.datetime(2017, 4, 13, 17, 10, 22, 378824), 'name': 'Arizona', 'id': 'af14c85b-172f-4474-8a30-d4ec21f9795e', 'created_at': datetime.datetime(2017, 4, 13, 17, 10, 22, 378763)}
guillaume@ubuntu:~/AirBnB_v3$ 
```

  * [models/engine/db_storage.py](./models/engine/db_storage.py)
  * [models/engine/file_storage.py](./models/engine/file_storage.py)
  * [tests/test_models/test_engine/test_db_storage.py](./tests/test_models/test_engine/test_db_storage.py)
  * [tests/test_models/test_engine/test_file_storage](./tests/test_models/test_engine/test_file_storage)

**3. Status of your API**

It’s time to start your API!

Your first endpoint (route) will be to return the status of your API:

```
guillaume@ubuntu:~/AirBnB_v3$ HBNB_MYSQL_USER=hbnb_dev HBNB_MYSQL_PWD=hbnb_dev_pwd HBNB_MYSQL_HOST=localhost HBNB_MYSQL_DB=hbnb_dev_db HBNB_TYPE_STORAGE=db HBNB_API_HOST=0.0.0.0 HBNB_API_PORT=5000 python3 -m api.v1.app
 * Running on http://0.0.0.0:5000/ (Press CTRL+C to quit)
...
```
In another terminal:

```
guillaume@ubuntu:~/AirBnB_v3$ curl -X GET http://0.0.0.0:5000/api/v1/status
{
  "status": "OK"
}
guillaume@ubuntu:~/AirBnB_v3$ 
guillaume@ubuntu:~/AirBnB_v3$ curl -X GET -s http://0.0.0.0:5000/api/v1/status -vvv 2>&1 | grep Content-Type
< Content-Type: application/json
guillaume@ubuntu:~/AirBnB_v3$ 
```

Magic right? (No need to have a pretty rendered output, it’s a JSON, only the structure is important)

Ok, let starts:

  * Create a folder `api` at the root of the project with an empty file `__init__.py`
  * Create a folder v1 inside api:
    * create an empty file `__init__.py`
    * create a file `app.py`:
      * create a variable `app`, instance of `Flask`
      * import `storage` from `models`
      * import `app_views` from `api.v1.views`
      * register the blueprint `app_views` to your Flask instance app
      * declare a method to handle `@app.teardown_appcontext` that calls `storage.close()`
      * inside `if __name__ == "__main__":`, run your Flask server (variable `app`) with:
        * host = environment variable `HBNB_API_HOST` or `0.0.0.0` if not defined
        * port = environment variable `HBNB_API_PORT` or `5000` if not defined
        * `threaded=True`
  * Create a folder `views` inside `v1`:
    * create a file __init__.py:
      * import `Blueprint` from `flask` doc
      * create a variable `app_views` which is an instance of `Blueprint` (url prefix must be `/ap1i/v1`)
      * wildcard import of everything in the package `api.v1.views.index` => PEP8 will complain about it, don’t worry, it’s normal and this file (`v1/views/__init__.py`) won’t be check.
    * create a file `index.py`
      * import `app_views` from `api.v1.views`
      * create a route `/status` on the object `app_views` that returns a JSON: `"status": "OK"` (see example)

  * [api/__init__.py](./api/__init__.py)
  * [api/v1/__init__.py](./api/v1/__init__.py)
  * [api/v1/views/__init__.py](./api/v1/views/__init__.py)
  * [api/v1/views/index.py](./api/v1/views/index.py)
  * [api/v1/app.py](./api/v1/app.py)

**4. Some stats?**

Create an endpoint that retrieves the number of each objects by type:

  * in `api/v1/views/index.py`
  * Route: `/api/v1/stats`
  * You must use the newly added `count()` method from `storage`

```
guillaume@ubuntu:~/AirBnB_v3$ curl -X GET http://0.0.0.0:5000/api/v1/stats
{
  "amenities": 47, 
  "cities": 36, 
  "places": 154, 
  "reviews": 718, 
  "states": 27, 
  "users": 31
}
guillaume@ubuntu:~/AirBnB_v3$ 
```

  * [api/v1/views/index.py](./api/v1/views/index.py)

**5. Not found**

Designers are really creative when they have to design a “404 page”, a “Not found”[… 34 brilliantly designed 404 error pages](https://intranet.alxswe.com/rltoken/8NwELW0j77kZ1jTM6hJFhA)

Today it’s different, because you won’t use HTML and CSS, but JSON!

In `api/v1/app.py`, create a handler for `404` errors that returns a JSON-formatted `404` status code response. The content should be: `"error": "Not found"`

```
guillaume@ubuntu:~/AirBnB_v3$ curl -X GET http://0.0.0.0:5000/api/v1/nop
{
  "error": "Not found"
}
guillaume@ubuntu:~/AirBnB_v3$ curl -X GET http://0.0.0.0:5000/api/v1/nop -vvv
*   Trying 0.0.0.0...
* TCP_NODELAY set
* Connected to 0.0.0.0 (127.0.0.1) port 5000 (#0)
> GET /api/v1/nop HTTP/1.1
> Host: 0.0.0.0:5000
> User-Agent: curl/7.51.0
> Accept: */*
> 
* HTTP 1.0, assume close after body
< HTTP/1.0 404 NOT FOUND
< Content-Type: application/json
< Content-Length: 27
< Server: Werkzeug/0.12.1 Python/3.4.3
< Date: Fri, 14 Apr 2017 23:43:24 GMT
< 
{
  "error": "Not found"
}
guillaume@ubuntu:~/AirBnB_v3$ 
```

  * [api/v1/app.py](./api/v1/app.py)

**6. State**

Create a new view for State objects that handles all default RESTFul API actions:

  * In the file `api/v1/views/states.py`
  * You must use `to_dict()` to retrieve an object into a valid JSON
  * Update `api/v1/views/__init__.py` to import this new file

Retrieves the list of all `State` objects: `GET /api/v1/states`

Retrieves a `State` object: `GET /api/v1/states/<state_id>`

  * If the `state_id` is not linked to any `State` object, raise a `404` error

Deletes a `State` object:: `DELETE /api/v1/states/<state_id>`

  * If the `state_id` is not linked to any `State` object, raise a `404` error
  * Returns an empty dictionary with the status code `200`

Creates a `State`: `POST /api/v1/states`

  * You must use `request.get_json` from Flask to transform the HTTP body request to a dictionary
  * If the HTTP body request is not valid JSON, raise a `400` error with the message `Not a JSON`
  * If the dictionary doesn’t contain the key `name`, raise a `400` error with the message `Missing name`
  * Returns the new `State` with the status code `201`

Updates a `State` object: `PUT /api/v1/states/<state_id>`

  * If the `state_id` is not linked to any `State` object, raise a `404` error
  * You must use `request.get_json` from Flask to transform the HTTP body request to a dictionary
  * If the HTTP body request is not valid JSON, raise a `400` error with the message `Not a JSON`
  * Update the State object with all key-value pairs of the dictionary.
  * Ignore keys: `id`, `created_at` and `updated_at`
  * Returns the `State` object with the status code `200`

```
guillaume@ubuntu:~/AirBnB_v3$ curl -X GET http://0.0.0.0:5000/api/v1/states/
[
  {
    "__class__": "State", 
    "created_at": "2017-04-14T00:00:02", 
    "id": "8f165686-c98d-46d9-87d9-d6059ade2d99", 
    "name": "Louisiana", 
    "updated_at": "2017-04-14T00:00:02"
  }, 
  {
    "__class__": "State", 
    "created_at": "2017-04-14T16:21:42", 
    "id": "1a9c29c7-e39c-4840-b5f9-74310b34f269", 
    "name": "Arizona", 
    "updated_at": "2017-04-14T16:21:42"
  }, 
...
guillaume@ubuntu:~/AirBnB_v3$ 
guillaume@ubuntu:~/AirBnB_v3$ curl -X GET http://0.0.0.0:5000/api/v1/states/8f165686-c98d-46d9-87d9-d6059ade2d99
 {
  "__class__": "State", 
  "created_at": "2017-04-14T00:00:02", 
  "id": "8f165686-c98d-46d9-87d9-d6059ade2d99", 
  "name": "Louisiana", 
  "updated_at": "2017-04-14T00:00:02"
}
guillaume@ubuntu:~/AirBnB_v3$ 
guillaume@ubuntu:~/AirBnB_v3$ curl -X POST http://0.0.0.0:5000/api/v1/states/ -H "Content-Type: application/json" -d '{"name": "California"}' -vvv
*   Trying 0.0.0.0...
* TCP_NODELAY set
* Connected to 0.0.0.0 (127.0.0.1) port 5000 (#0)
> POST /api/v1/states/ HTTP/1.1
> Host: 0.0.0.0:5000
> User-Agent: curl/7.51.0
> Accept: */*
> Content-Type: application/json
> Content-Length: 22
> 
* upload completely sent off: 22 out of 22 bytes
* HTTP 1.0, assume close after body
< HTTP/1.0 201 CREATED
< Content-Type: application/json
< Content-Length: 195
< Server: Werkzeug/0.12.1 Python/3.4.3
< Date: Sat, 15 Apr 2017 01:30:27 GMT
< 
{
  "__class__": "State", 
  "created_at": "2017-04-15T01:30:27.557877", 
  "id": "feadaa73-9e4b-4514-905b-8253f36b46f6", 
  "name": "California", 
  "updated_at": "2017-04-15T01:30:27.558081"
}
* Curl_http_done: called premature == 0
* Closing connection 0
guillaume@ubuntu:~/AirBnB_v3$ 
guillaume@ubuntu:~/AirBnB_v3$ curl -X PUT http://0.0.0.0:5000/api/v1/states/feadaa73-9e4b-4514-905b-8253f36b46f6 -H "Content-Type: application/json" -d '{"name": "California is so cool"}'
{
  "__class__": "State", 
  "created_at": "2017-04-15T01:30:28", 
  "id": "feadaa73-9e4b-4514-905b-8253f36b46f6", 
  "name": "California is so cool", 
  "updated_at": "2017-04-15T01:51:08.044996"
}
guillaume@ubuntu:~/AirBnB_v3$ 
guillaume@ubuntu:~/AirBnB_v3$ curl -X GET http://0.0.0.0:5000/api/v1/states/feadaa73-9e4b-4514-905b-8253f36b46f6
{
  "__class__": "State", 
  "created_at": "2017-04-15T01:30:28", 
  "id": "feadaa73-9e4b-4514-905b-8253f36b46f6", 
  "name": "California is so cool", 
  "updated_at": "2017-04-15T01:51:08"
}
guillaume@ubuntu:~/AirBnB_v3$ 
guillaume@ubuntu:~/AirBnB_v3$ curl -X DELETE http://0.0.0.0:5000/api/v1/states/feadaa73-9e4b-4514-905b-8253f36b46f6
{}
guillaume@ubuntu:~/AirBnB_v3$ 
guillaume@ubuntu:~/AirBnB_v3$ curl -X GET http://0.0.0.0:5000/api/v1/states/feadaa73-9e4b-4514-905b-8253f36b46f6
{
  "error": "Not found"
}
guillaume@ubuntu:~/AirBnB_v3$
```

