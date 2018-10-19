# GoogleCloudPlatformCommands
This note is describing a GCP tutorial by https://www.topgate.co.jp 
***

### 【GCP入門編・第2回】まずは、ここから！知らないと恥ずかしい Google Cloud Platform (GCP) の事前準備！
https://www.topgate.co.jp/gcp02-getting-started-guide  
> just creating an account on GCP
***

### 【GCP入門編・第3回】難しくない！ Google Compute Engine (GCE) でのインスタンス起動方法！  
https://www.topgate.co.jp/gcp03-google-compute-engine-launch-instance  
> make an instance on Google Compute Engine (GCE). This is my instance. 
![image](https://user-images.githubusercontent.com/6435299/47217747-519c4480-d3e4-11e8-8085-4eeb3d5543f7.png)


> download Cloud SDK to log into the created instance from the local environment
> "gcloud init" to start the initial configuration
> 

The official reference on how to access the virtual environment on GCE from the local env is written here.
https://cloud.google.com/sdk/docs/quickstart-windows

***

how to run a simple app on GCP
https://www.topgate.co.jp/gcp04-google-compute-engine-run-application　
used commands

```
$ git clone https://github.com/GoogleCloudPlatform/python-docs-samples.git

$ cp -rf appengine/standard/flask/hello_world hello_world
$ cd hello_world
$ git init .
$ git add .
$ git commit -m “Initial Commit”
```

the state that an empy repository is created
![image](https://user-images.githubusercontent.com/6435299/46917214-4c638200-cfff-11e8-8cdd-e927d44fafa7.png)
```
$ gcloud init && git config --global credential.https://source.developers.google.com.helper gc
$ git remote add google \
  https://source.developers.google.com/p/tidal-geode-182004/r/myRepository1
$ git push --all google  
```




