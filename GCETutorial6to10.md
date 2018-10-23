

***

### 【GCP入門編・第6回】これは簡単！ Google App Engine での Cloud Datastore の利用方法！
https://www.topgate.co.jp/gcp06-how-to-use-cloud-datastore-gae

In this section, we make a simple task management application using Cloud Datastore based on the hello-world application that we made in the last section.  

Cloud Datastore is a NoSQL-base database such as MongoDB and RethinkDB.

- 1. modify main.py in hello_world directory in google cloud SDK like this.
```
# Copyright 2016 Google Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
# [START app]
import logging
from flask import Flask, redirect, request, render_template
from google.appengine.ext import ndb
class Message(ndb.Model):
    body = ndb.StringProperty()
    created = ndb.DateTimeProperty(auto_now_add=True)
app = Flask(__name__)
@app.route('/')
def hello():
    # Get message list
    messages = Message.query().fetch()
    return render_template('hello.html', messages=messages)
@app.route('/add', methods=['POST'])
def add_message():
    message_body = request.form.get('message', '')
    message = Message(body=message_body)
    message.put()
    return redirect('/')
@app.errorhandler(500)
def server_error(e):
    # Log the error and stacktrace.
    logging.exception('An error occurred during a request.')
    return 'An internal error occurred.', 500
```
    
- 2. add a HTML template where we input messages and show them.
```
// add ”templates” directory and create templates/hello.html” as shown below.
<!doctype html>
<head>
  <title>Message Board</title>
</head>
<body>
  <form action="/add" method="post">
    <textarea name="message" id="message_area"></textarea>
    <input type="submit"></input>
  </form>
  <ul>
    {% for message in messages %}
    <li>{{ message['body'] }} at {{ message['created'] }}</li>
    {% endfor %}
  </ul>
</body>
```


    
    
