# GoogleCloudPlatformTutorials 2

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
    
- 2. add a HTML template where we input messages and show them. add ”templates” directory and create templates/hello.html” as shown below.
```
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

- 3. execute the following commands. 
```
$ gcloud app deploy
$ gcloud app browse    
```

![image](https://user-images.githubusercontent.com/6435299/47373150-3011c480-d726-11e8-9fca-b53176dbd516.png)

![image](https://user-images.githubusercontent.com/6435299/47373392-b201ed80-d726-11e8-97a6-b81bbe8f7f65.png)

We found that the suggested way did not work, showing the error message above. We need to look for other ways.

***
【GCP入門編・第7回】知らなきゃ損！ Google Container Engine (GKE) での Dockerイメージを使ったコンテナの起動方法！
https://www.topgate.co.jp/gcp07-how-to-start-docker-image-gke

In this section, we establish Docker container using Google Container Engine (GKE).

After enabling Kubernetes Engine API on API Library, we create a container cluster.

![image](https://user-images.githubusercontent.com/6435299/47612573-e24be200-dac0-11e8-8aab-61a21eb73e98.png)

![image](https://user-images.githubusercontent.com/6435299/47612806-86378c80-dac5-11e8-9c17-66452e4996e0.png)

With "gcloud compute instances list" command on the local terminal (or command prompt for Windows), you can see that the containers were created (in this case, three containers were created becasue I chose making 3 nodes)

![image](https://user-images.githubusercontent.com/6435299/47612831-f0e8c800-dac5-11e8-9ae1-3d1577346d11.png)

Next, run Docker containers on the cluster(s).

- install kubectl (a command line tool to manipulate Kubarnetes). To do it, start gcloud SDK as an administra then run this command
```
gcloud components install kubectl
```
![image](https://user-images.githubusercontent.com/6435299/47612863-df53f000-dac6-11e8-8ed8-d4f21059b1db.png)

![image](https://user-images.githubusercontent.com/6435299/47612879-38bc1f00-dac7-11e8-8772-82cbb8c9ed17.png)

- set the environemnt variable and the default zone to let kubectl recognize Container Engine clusters.
```
$ export PROJECT_ID="your project id" // this command worked only on Git, not the command prompt or google SDK.
$ gcloud config set compute/zone asia-northeast1-a
```

![image](https://user-images.githubusercontent.com/6435299/47613113-f812d480-dacb-11e8-98a8-4b032edcdac3.png)

- get the authentication info to let jubectl access the clusters. 
```
$ gcloud auth application-default login
$ gcloud container clusters get-credentials "your cluster name" // in my case "your cluster name" is cluster-1	
```
![image](https://user-images.githubusercontent.com/6435299/47613101-a9653a80-dacb-11e8-8e29-d7f3ade97450.png)

- establish a pod using kubectl by running the following command (WordPress Image (tutum/wordpress) is gonna execute).
```
$ kubectl run wordpress --image=tutum/wordpress --port=80
```
![image](https://user-images.githubusercontent.com/6435299/47613124-20023800-dacc-11e8-8c6c-6f8c98d3a05a.png)

- you can view the deployment info with the following command.
```
$ kubectl describe deployments
```
![image](https://user-images.githubusercontent.com/6435299/47613136-4de77c80-dacc-11e8-8459-0729b950f012.png)

- you cannot access the deployed WordPress in the current state. the pod has an IP address, but it is only for internal accesses. we need to enable external accesses via LoadBalancer by making a service. we conduct this configuration by the following command on kubectl.
```
$  kubectl expose deployment wordpress --type=LoadBalancer
```
![image](https://user-images.githubusercontent.com/6435299/47613242-c0f1f280-dace-11e8-9c7f-c498dbcfd3ef.png)

- we can finally open the service to the public. check the status with this command. 
```
$ kubectl describe services wordpress //endpoint is the external IP address. IP is the internal IP address.
```
![image](https://user-images.githubusercontent.com/6435299/47613248-f4348180-dace-11e8-9a75-36f630c0c891.png)

![image](https://user-images.githubusercontent.com/6435299/47613322-72455800-dad0-11e8-8884-1180066e9830.png)

- not to let others use the opened wordpress, dont forget delete the Service and Deployment
```
$ kubectl delete services wordpress
$ kubectl delete deployment wordpress
```
![image](https://user-images.githubusercontent.com/6435299/47613513-f9e09600-dad3-11e8-98ec-35433d6aa94b.png)

### run more than one containers with Container Engine

- in GKE, we can easily run more than one container. Here is an example where we run two ngnxes.
```
$ kubectl run nginx --image=nginx --replicas=2
```
![image](https://user-images.githubusercontent.com/6435299/47613789-79706400-dad8-11e8-9374-7210bf5c6364.png)

- we can see that two pods are running with the following command.
```
// watching running pods
$ kubectl get pods
```
![image](https://user-images.githubusercontent.com/6435299/47613796-a3c22180-dad8-11e8-9bd6-14b69a17cf2f.png)

- make a service as a LoadBalancer.
```
$ kubectl expose deployment nginx --type=LoadBalancer
```

ここから

describe services コマンドで IP を確認します。

$ kubectl describe services


ブラウザで LoadBalancer Ingress に表示された IP を開くと、 nginx が起動していることがわかります。

このように、 GKE では簡単に複数のコンテナを立ち上げることができ、アクセスを LoadBalancer で振り分ける、といったことも可能です。
先ほどと同様、 Deployment と Service を削除します。

$ kubectl delete services nginx
20.jpg

$ kubectl delete deployments nginx
21.jpg
最後に、クラスターを終了します。クラスターの終了には gcloud コマンドを使用します。

$ gcloud container clusters delete クラスタ-名





































