数据库编程
-------------

  Python数据库API定义了一组用于连接数据库服务器、执行SQL语句的高级函数和对象。主要有两个主要对象：
    * 管理数据库连接的Connection对象
    * 执行SQL语句的Cursor对象

1. Oracle数据库编程
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- **Oracle动态根据输入环境的参数执行特定数据库查询操作**

.. code:: python

    # -*-coding:utf8-*-
    """
    Author: Wang Shaofei
    Created: 2019-7-1
    Purpose: Oracle connection function
             Use to query data from oracle database dynamically
    """


    import cx_Oracle
    from pprint import pprint
    import random


    class Oracle:

        def __init__(self, user, password, tnsname):
            self.user = user # Oracle数据库用户名
            self.password = password # Oracle数据库密码
            self.tns = tnsname # Oracle数据库实例连接tnsname
            if user.upper() == 'SYS':
                self.q = cx_Oracle.connect(self.user, self.password, self.tns, cx_Oracle.SYSDBA)
            else:
                self.q = cx_Oracle.connect(self.user, self.password, self.tns)

        def q_data(self, sql):
            try:
                with self.q.cursor() as cursor:
                    cursor.execute(sql)
                    return cursor.fetchall()

            except cx_Oracle.DatabaseError as e:
                print('No Data Found or Oracle Database can not connect or can not connect using SYSDBA mode!\n ', e)



- **Python RESTful API 与Oracle APEX**

    已实现Python Flask RESTful API程序原型样例。

.. code:: python

    # -*- coding: utf-8 -*-
    """
    创建日期：2018-9-17
    用法: http://127.0.0.1:5000/?input_text=安全身份验证
    内容: 与机器学习预测程序包一起使用，对外提供预测服务
    作者：Shaofei Wang
    QQ: 47181770
    """

    from flask import Flask
    from flask_restful import Resource, Api, reqparse
    from pinlp import train_model_NB


    app = Flask(__name__)
    api = Api(app)

    parser = reqparse.RequestParser()
    parser.add_argument('input_text', type=str, location='args')


    class Prediction(Resource):
        def get(self):
            args = parser.parse_args()
            print('voice input text is {0}'.format(args))
            input_text = args['input_text']
            print('voice input text is {0}'.format(input_text))
            predicted = train_model_NB(input_text) # 根据输入语音使用模型进行预测
            print('predicted type is *** {0}'.format(type(predicted)))
            prediction = str(predicted)
            print('My Prediction is {0}'.format(prediction))
            return prediction


    api.add_resource(Prediction, '/')

    if __name__ == '__main__':
        app.run(host='0.0.0.0',port=8006)

-----------------------------------------


2. MySQL数据库编程
~~~~~~~~~~~~~~~~~~~~

- **MySQL动态根据输入环境的参数执行特定数据库查询操作**

.. code:: python

    # -*-coding:utf8-*-
    """
    Author: Shaofei Wang
    Created: 2019-3-1
    Purpose: MySQL connection function
             Use to query data from MYSQL database dynamically
    """

    import pymysql # 导入模块
    from pprint import pprint


    class Mysql:
        def __init__(self, host, user, password, db, port=3306, data_type="dic"):
            connect_arg = {"host":host, # Mysql数据库服务器IP
                         "user":user, # Mysql数据库用户名
                         "password":password, # Mysql数据库密码
                         "db":db, # Mysql数据库实例名
                         "port": port, # Mysql数据库实例端口
                         "charset":'utf8mb4',
                         "cursorclass": pymysql.cursors.DictCursor
            }
            self.host = host
            if data_type == "list":
                connect_arg.pop("cursorclass")
            self.q = pymysql.connect(**connect_arg)

        def q_data(self, sql):
            with self.q.cursor() as cursor:
                cursor.execute(sql)

                return cursor.fetchall()
            self.q.close()

    # 调用并测试q_data执行SQL查询
    # ip_address = 'x.x.x.x'
    # db_username = 'root'
    # password = 'root'
    # port = 3306
    # ms = Mysql(str(ip_address), str(db_username), str(password), 'information_schema', int(port), 'list')
    # lock_count = ms.q_data("select count(*) from INFORMATION_SCHEMA.INNODB_LOCKS;")
    #
    # pprint(lock_count)


-----------------------------------------


3. Influxdb时序数据库编程
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


- **数据Influxdb连接**

.. code:: python

    from influxdb import InfluxDBClient
    from influxdb import SeriesHelper

    # InfluxDB connections settings
    host = 'x.x.x.x'
    port = 8086
    user = 'root'
    password = 'root'
    dbname = 'mydb'

    myclient = InfluxDBClient(host, port, user, password, dbname)

    class MySeriesHelper(SeriesHelper):

        # Meta class stores time series helper configuration.


        class Meta:

            # The client should be the instance of InfluxDBClient

            client = myclient

            # The series name must be a string. Add dependent fields and tags in curly brackets.
            series_name = ''
            # Defines all the fields in this time series.
            fields = []
            # Defines all the tags for the series.
            tags = []
            # Defines the number of data points to store prior to writing on the wire.
            bulk_size = 1
            # autocommit must be set to True when using bulk_size
            autocommit = True


- **Grafana与Influxdb**
   时序数据插入influxdb后，在Grafana配置界面>配置数据源>influxdb选件 输入相关参数，即可自定制监控页面。

-----------------------------------------


4. Python for Hadoop数据库编程
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- **数据库连接**


- **数据库操作**


- **Hadoop 与Apache Kylin**


5. Python for API 数据编程
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- **从网上RESTful API获取数据**

  获取的数据格式转换为json格式，通过字典读取获取相应的数值
  
.. code:: python

 # -*- coding: utf-8 -*-
 import requests
 import json
 import datetime


 def get_tx_fy():

     url = 'https://view.inews.qq.com/g2/getOnsInfo?name=disease_h5'
     result = requests.get(url)
     res_json = json.loads(result.text)
     print(res_json)
     data = json.loads(res_json['data'])
     chinaTotal = data['chinaTotal']
     chinaadd = data['chinaAdd']
     lastupdate = data['lastUpdateTime']

     print('更新时间为 {} '.format(lastupdate))
     print('现在比上一日增：确诊人数{}， 疑似人数{}， 死亡人数{}，治愈人数{} '.format(chinaadd['confirm'], chinaadd['suspect'], chinaadd['dead'],chinaadd['heal']))
     print('确诊总人数为{}， 总疑似人数{}， 总死亡人数{}， 总治愈人数{}'.format(chinaTotal['confirm'], chinaTotal['suspect'], chinaTotal['dead'], chinaTotal['heal']))

     return lastupdate, chinaadd['confirm'], chinaadd['suspect'], chinaadd['dead'], chinaadd['heal'], chinaTotal['confirm'], chinaTotal['suspect'], chinaTotal['dead'], chinaTotal['heal']


 feiyan = get_tx_fy()
 last_date = datetime.datetime.now().strftime('%Y-%m-%d %H:%M:%S')
 print(feiyan)



参考：

 1. 使用 Python 和 Flask 设计 RESTful API, `flask_RESTfulAPI`_
 2. 使用Oracle APEX与Flask RESTful API集成, `oracleApex_RESTful_Integration`_


.. _flask_RESTfulAPI: http://www.pythondoc.com/flask-restful/first.html

.. _oracleApex_RESTful_Integration: https://www.oracle.com/technetwork/cn/articles/dsl/mastering-oracle-python-soa-1391432-zhs.html
