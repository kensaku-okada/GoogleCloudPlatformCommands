# GoogleCloudPlatformCommands
This note is describing a GCP tutorial by https://www.topgate.co.jp 
***

### 【GCP入門編・第2回】まずは、ここから！知らないと恥ずかしい Google Cloud Platform (GCP) の事前準備！
https://www.topgate.co.jp/gcp02-getting-started-guide  
- just creating an account on GCP
***

### 【GCP入門編・第3回】難しくない！ Google Compute Engine (GCE) でのインスタンス起動方法！  
https://www.topgate.co.jp/gcp03-google-compute-engine-launch-instance  
- make an instance on Google Compute Engine (GCE). This is my instance. 
![image](https://user-images.githubusercontent.com/6435299/47217747-519c4480-d3e4-11e8-8085-4eeb3d5543f7.png)

- if you click "SSH" button above, you can start the created virtual environment like this.
![image](https://user-images.githubusercontent.com/6435299/47218066-82c94480-d3e5-11e8-8c75-4319fc85cbc0.png)


- to log into the created instance from the local environment, download Cloud SDK (https://cloud.google.com/sdk/). 
- open google clould SDK, and "gcloud init" to start the initial configuration
- if required, set the default time zon like "gcloud config set compute/zone asia-northeast1-a" 
- clicking the triangle button next to "SSH" and ”View gcloud command”, you get the command to log in the instance from the local like this.
![image](https://user-images.githubusercontent.com/6435299/47218018-5a414a80-d3e5-11e8-8136-ff90dd6e777a.png)

- inserting the command on the google cloud SDK, you can log in the instance (virtual env)
![image](https://user-images.githubusercontent.com/6435299/47219493-e8b7cb00-d3e9-11e8-99f3-badc234f76a5.png)

- install nginx by "sudo apt-get install nginx"
- after the installation, go back to GCP console, and go to the external IP address, then you gonna see the page like this.
![image](https://user-images.githubusercontent.com/6435299/47219710-8f03d080-d3ea-11e8-96b6-b5ea4d62f3f7.png)


The official reference on how to access the virtual environment on GCE from the local env is written here.
https://cloud.google.com/sdk/docs/quickstart-windows

***
### 【GCP入門編・第4回】すぐ出来なくても大丈夫！サンプルアプリで Google Compute Engine (GCE) の動作練習！
https://www.topgate.co.jp/gcp04-google-compute-engine-run-application
In this section, we just run a sample application on Google Compute Engine (GCE). It was assumed that you have Git on the local machine.

- used commands
```
//Just download the sample app. we do not use all of the source code
$ git clone https://github.com/GoogleCloudPlatform/python-docs-samples.git
// we use only ”appengine/standard/flask/hello_world” in "python-docs-samples" folder
$ cp -rf appengine/standard/flask/hello_world hello_world
$ cd hello_world
$ git init .
$ git add .
$ git commit -m “Initial Commit”
```

- open GCP and move to "Source Repositories" and create an empty repository.
![image](https://user-images.githubusercontent.com/6435299/47219997-69c39200-d3eb-11e8-8602-a613515185d4.png)
- click  ”Push code from a local Git repository to your Cloud Repository” and run the shown command 

- the state that an empty repository is created
![image](https://user-images.githubusercontent.com/6435299/46917214-4c638200-cfff-11e8-8cdd-e927d44fafa7.png)

- run these command on your google cloud SDK shell. these does not work with Git Bash on the local environemnt. We push the local git repository to the GCE instance.
```
$ gcloud init && git config --global credential.https://source.developers.google.com.helper gcloud.cmd
$ git remote add google \
  https://source.developers.google.com/p/'your project name'/r/myRepository1
$ git push --all google  
```
![image](https://user-images.githubusercontent.com/6435299/47220151-e8203400-d3eb-11e8-970a-7d58503da428.png)

- Here are the outputs when executing the commands.
![image](https://user-images.githubusercontent.com/6435299/47220897-fc653080-d3ed-11e8-85db-def8a429a86c.png)

- **When you have an error "fatal: remote google already exists." at the second command, do "git remote rm google", then it will proceed correctly.**
![image](https://user-images.githubusercontent.com/6435299/47225178-6be01d80-d3f8-11e8-8eb8-790a6323d81e.png)

- if you successfully push the repository, you gonna see the directories and files on the GCP browser like this (in this picture, I pushed all the directories of "python-docs-samples".)
![image](https://user-images.githubusercontent.com/6435299/47225321-c5484c80-d3f8-11e8-8d0a-0683a949498f.png)

- you finished registering the private repository on the remote. Next, run a sample application on GCE instance.
```
// log in the instance through google cloud SDK. In my case, the instance name is myinstance-1
$ gcloud compute ssh "your instance name"
// the VM console opens
// clone the repository to this instance
$ gcloud init
$ gcloud source repos clone "your repository name" --project="your project ID" 
```
If you get the error "ERROR: (gcloud.source.repos.clone) NOT_FOUND: Requested entity was not found.", change the VM's Cloud API access scopes as follows.
![image](https://user-images.githubusercontent.com/6435299/47226468-91baf180-d3fb-11e8-9e2c-22d5ff1befb8.png)


![image](https://user-images.githubusercontent.com/6435299/47226700-1c035580-d3fc-11e8-9e42-8905cfdb2b5b.png)


![image](https://user-images.githubusercontent.com/6435299/47226723-2de4f880-d3fc-11e8-903d-f406aaa44568.png)


![image](https://user-images.githubusercontent.com/6435299/47227632-47873f80-d3fe-11e8-86c4-0437eb46d321.png)


if you are still running nginx, stop it by:
```
$ sudo service nginx stop
```
run “hello-world” application as below.
```
$ cd hello-world
$ sudo apt install python-pip gunicorn
$ pip install --upgrade pip
$ sudo pip install -r requirements.txt
$ sudo gunicorn -b 0.0.0.0:80 main:app
```
the original website says the commands above should show  “Hello World!”, but the following erros actually occured.
 
 ![image](https://user-images.githubusercontent.com/6435299/47229016-a00c0c00-d401-11e8-886e-a4b4686670d6.png)
This problem was solved by "python -m pip uninstall pip". (see https://stackoverflow.com/questions/49964093/file-usr-bin-pip-line-9-in-module-from-pip-import-main-importerror-canno/49988228)

![image](https://user-images.githubusercontent.com/6435299/47228899-5f13f780-d401-11e8-96d0-56eb9a0252a6.png)
Still requirements.txt could not be read. When I tried "sudo gunicorn -b 0.0.0.0:80 main:app" anyway, I got an error "ImportError: No module named flask". Then, I manually installed flask.

![image](https://user-images.githubusercontent.com/6435299/47229073-c7fb6f80-d401-11e8-92b4-bccab24d5641.png)
Still, this error occured.

***

### 【GCP入門編・第5回】 Google App Engine の魅力とは？ Google App Engine (GAE) でのアプリケーション起動方法！
https://www.topgate.co.jp/gcp05-google-app-engine-run-application
In progress.






