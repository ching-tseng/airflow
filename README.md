# airflow
嘗試自己安裝Airflow於Ubuntu 18.04 - Python3.6

##﻿Reference: https://medium.com/@cchangleo/airflow-新手建置測試-part-1-9e0fe4d12e7a


## 安裝Mysql 環境
```
sudo apt-get install mysql-server
sudo apt-get install mysql-client -y
sudo apt-get install libmysqlclient-dev
```

## 進入Mysql設定
```
sudo mysql -u root

mysql> show global variables like ‘%timestamp%’;
```
### 如果看到 explicit_defaults_for_timestamp狀態為OFF往下
```
mysql> set global explicit_defaults_for_timestamp=1;
mysql> show global variables like ‘%timestamp%’;
```
### 查看 explicit_defaults_for_timestamp狀態是否為ON

## 新增airflow使用者及資料表
```
mysql> create database airflow default charset utf8 collate utf8_general_ci;
mysql> create user airflow@'localhost' identified by 'airflow';
mysql> grant all on airflow.* to airflow@'localhost';
mysql> flush privileges;
```

## 更新環境
```
sudo pip3 install --upgrade pip
sudo apt-get install python3-pip
sudo pip install PyMySQL
```

### 此處因為Python3將ConfigParsre.py改成小寫，導致無法安裝mysql_python套件，所以複製一份改成大寫檔名
```
sudo cp /usr/local/lib/python3.6/dist-packages/configparser.py /usr/local/lib/python3.6/dist-packges/ConfigParser.py
```


## 安裝airflow
```
sudo pip install apache-airflow
sudo pip install ‘apache-airflow[postgres, mysql, slack, rabbitmq, celery, crypto]’
```

## 在家目錄底下新增資料夾
```
~> mkdir airflow && cd airflow
~/airflow> export AIRFLOW_HOME=$(pwd)
~/airflow> echo $AIRFLOW_HOME
~/airflow> export SLUGIFY_USES_TEXT_UNIDECODE=yes
~/airflow> airflow initdb
```

## 更改Config內容
```
~/airflow> sudo vi airflow.cfg
```

### 找到sql_alchemy_conn後面預設連接資料庫為sqlite3, 更改為Mysql連線API
```
sql_alchemy_conn = mysql+mysqldb://airflow:airflow@localhost:3306/airflow
```

## 更改完成後重新初始化airflow
```
~/airflow> airflow initdb
```

## 啟動airflow
```
~/airflow> airflow webserver -p 8080
```

# 完成
