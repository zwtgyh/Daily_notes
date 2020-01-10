### Tracker数据分析

#### 1.在log目录下新建每天的目录

* 名称格式形如 2019-11-11

#### 2.收集ECU的数据并处理

* 捞取`ECU`数据，每个`ECU`的数据均打包成一个`zip`文件，以`ECU`编号后三位命名，入`017.zip`
* 将`zip`文件放入对应日期的目录，并在项目跟目录运行`python3 pre_handle_logs.py -t 2019-11-20`进行预处理（处理前记得修改脚本底部的日期）
* 处理结束后可以看到生成的新文件，每个`ECU`对应一个文件夹，文件夹名字以编号后三位命名，文件夹里边总共有三个`txt`文件，分别是`error_log.txt`,`parsed.txt`,`reboot_log.txt`

#### 3.分析日志

* 在根目录运行`python3 compare.py -t 2019-11-20`分析数据，可以看到最终分析出的收投车数量
* -t 指定日期
* -i 指定设备，后3位，与log zip文件名字保持一致，不指定则默认所有设备都处理
* -a 出来所有日志，如果指定，会忽略-t
* -p 处理从今天起往前 n 天的数据，默认值为-1，如果设置了，则忽略-t

#### 日志详情
    patyon time_analysis.py
    
#### 设备轨迹
    patyon server.py      map.html里更改get_path('2019-12-26', '212', true, '07:00:00', '19:00:00') 获取对应的轨迹地图
    
#### 4.字段解释

* parsed.txt 文件里边为每日设备的所有日志
* workload.csv 文件里边为每日运维工人的工作记录

##### a.parsed.txt

* `curTime`:上报的时间
* `longitude`和`latitude`:上报的经纬度
* `deviceInfo`:扫描到的蓝牙设备信息列表

##### b.`deviceInfo`里边每一个对象的字段

* `gpsTime`:扫到设备时取的经纬度的时间
* `scanTime`:扫到该蓝牙设备时的终端上的时间
* `scanLatitude`和`scanLongitude`:扫到设备时的经纬度
* `rssi`:蓝牙信号强度（不稳定）
* `address`:蓝牙mac地址

##### c.workload.csv字段说明

* `vid`:车辆id
* `mac`:蓝牙mac
* `eid`:ecu id后三位
* `type`:工作类型，on收车，off投车
* `t_create`:创建时间
* `t_modify`:修改时间

#### 4.TODO

* 进一步细化每个时段内日志分析数据和实际工作数据的对比 