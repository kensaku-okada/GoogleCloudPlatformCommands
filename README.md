# GoogleCloudPlatformCommands

how to access the virtual environment on GCE from the local env is written here.
https://cloud.google.com/sdk/docs/quickstart-windows

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




