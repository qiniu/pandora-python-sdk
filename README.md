Pandora SDK for Python
======================

概述
----

Pandora Python SDK 0.0.1版本。该版本依赖于第三方HTTP库 `requests <https://github.com/kennethreitz/requests>`

运行环境
--------
建议使用Python 2.7

快速使用
--------

.. code-block:: python

    # -*- coding: utf-8 -*-

	from pandora import api
	from pandora.models import *
	from pandora.utils import *

    endpoint = 'http://pipeline.qiniu.com'
    repo_name = `<你的消息队列名称>` # 假设你的消息队列有三个字段，分别是string类型的f1，long类型的f2，float类型的f3

	client = api.Client(endpoint, `<你的AccessKey>`, `<你的SecretKey>`)
    # 从string中上传数据打点
    client.post_data_from_string(repo_name, 'f1="x"\tf2=1234\tf3=1.23')

    # 构造数据点上传数据打点
	f1 = Field("f1", "v1")
	f2 = Field("f2", 330709)
	f3 = Field("f3", 2.2223)
	point = Point([f1, f1])
	client.post_data(repo_name,[point])

    # 从元组中构造数据点上传打点
	p1 = [("f1", "v1"), ("f2", 1234567890), ("f3", 3.14)]
	p2 = [("f1", "v2"), ("f2", 9876543210), ("f3", 1.414)]
	client.post_data(repo_name, [to_point(p1), to_point(p2)])

出错处理
--------

一旦出错，Python SDK的接口直接抛出异常，具体的异常定义在pandora.exceptions子模块中。参考例子如下：

.. code-block:: python

    try:
        client.post_data(repo_name, points)
    except pandora.exceptions.RequestError as e:
        print('post data failed: http_status={0}, request_id={1}, message={2}'.format(e.status, e.request_id, e.message))

