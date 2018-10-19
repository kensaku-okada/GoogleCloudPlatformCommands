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


![image](https://user-images.githubusercontent.com/6435299/47220897-fc653080-d3ed-11e8-85db-def8a429a86c.png)










