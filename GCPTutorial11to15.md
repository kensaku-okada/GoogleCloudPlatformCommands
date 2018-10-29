# GoogleCloudPlatformTutorials 3

***

### 【GCP入門編・第11回】 Google Cloud Dataproc を使ってデータを解析しよう！


in progress.



***

### 【GCP入門編・第12回】 BigQuery を使って気軽にビッグデータの解析を行ってみよう！

https://www.topgate.co.jp/gcp12-how-to-analyze-big-data-with-bigquery

In this section, we import a CSV data to BigQuery and make a simple anaysis.

- download the data
download the caompany data in Tokyo as unicode csv from thie link.　
http://www.houjin-bangou.nta.go.jp/download/zenken/#csv-unicode

![image](https://user-images.githubusercontent.com/6435299/47645075-464ed300-dbb3-11e8-9ffb-ffd839b85cd3.png)


- unzip the file and upload it to your google drive account.
![image](https://user-images.githubusercontent.com/6435299/47645034-2ddeb880-dbb3-11e8-884e-4930367fc5a7.png)

- get the link of the csv file.
![image](https://user-images.githubusercontent.com/6435299/47645301-f1f82300-dbb3-11e8-9b22-b204573802e0.png)

- moving on to GCP, click "BigQuery" and 

![image](https://user-images.githubusercontent.com/6435299/47645368-2966cf80-dbb4-11e8-9d3e-cfce418983fc.png)

- go to "Go To Classic UI".
![image](https://user-images.githubusercontent.com/6435299/47648021-1b1cb180-dbbc-11e8-855b-f054215c16b6.png)

- switch to the old UI and click "create new dataset".
![image](https://user-images.githubusercontent.com/6435299/47647348-0f2ff000-dbba-11e8-8ba1-75198fc1caae.png)

- set like this.
![image](https://user-images.githubusercontent.com/6435299/47648234-c594d480-dbbc-11e8-880a-8cd57debaa3e.png)

- next, create new table.
![image](https://user-images.githubusercontent.com/6435299/47648298-f7a63680-dbbc-11e8-9fa9-a0f723266970.png)
define the table like this.
![image](https://user-images.githubusercontent.com/6435299/47649697-67b6bb80-dbc1-11e8-9d91-d53cfa4cfcc9.png)

- clikd query table and run a SQL like this:
```
SELECT count(name) FROM ["your project id":tokyo_companies.company] WHERE city = "千代田区"
```
For just making sure if the table is created, you could also run a simple code like this:
```
SELECT * FROM ["your project id":tokyo_companies.company] LIMIT 10
```
![image](https://user-images.githubusercontent.com/6435299/47649783-a3ea1c00-dbc1-11e8-9428-5b1393a6eb28.png)

- more than 1 million search finished by 47.3 seconds.
![image](https://user-images.githubusercontent.com/6435299/47650466-ae0d1a00-dbc3-11e8-9372-64ec72fdb4ae.png)

- a bit more complicated query was run below.
```
SELECT city, count(company_number) AS company_number FROM ["your project id":tokyo_companies.company] GROUP BY city　ORDER BY company_number DESC
```
![image](https://user-images.githubusercontent.com/6435299/47650414-83bb5c80-dbc3-11e8-8696-a828a90ada47.png)



















