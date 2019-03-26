# 省市区数据采集并标注拼音

当前最新版为`2019文件夹`内的`2018版数据`，此数据发布于`2019-01-31`。

可直接打开`采集到的数据`文件夹内的`ok_data_level4.csv`来使用，level4是省市区镇4级数据，level3是省市区3级数据。另外不需要的数据可以简单的用Excel筛选后直接删除。csv格式非常方便解析或导入数据库。


## 采集环境

chrome 控制台，`41.0.2272.118`这版本蛮好，新版本乱码、SwitchyOmega代理没有效果、各种问题（[简单制作chrome便携版实现多版本共存](https://github.com/xiangyuecn/Docs/blob/master/Other/%E8%87%AA%E5%B7%B1%E5%88%B6%E4%BD%9Cchrome%E4%BE%BF%E6%90%BA%E7%89%88%E5%AE%9E%E7%8E%B0%E5%A4%9A%E7%89%88%E6%9C%AC%E5%85%B1%E5%AD%98.md)）


## 数据源

国家统计局 > 统计数据 > 统计标准 > [统计用区划和城乡划分代码](http://www.stats.gov.cn/tjsj/tjbz/tjyqhdmhcxhfdm/)


## 采集深度

- 2019文件夹采集了4层，省、市、区、镇，[2018版数据](http://www.stats.gov.cn/tjsj/tjbz/tjyqhdmhcxhfdm/2018/index.html)。
- 2018文件夹采集了3层，省、市、区，[2017版数据](http://www.stats.gov.cn/tjsj/tjbz/tjyqhdmhcxhfdm/2017/index.html)。
- 2017文件夹采集了3层，省、市、区，[2016版数据](http://www.stats.gov.cn/tjsj/tjbz/tjyqhdmhcxhfdm/2016/index.html)。
- 2013文件夹采集了4层，省、市、区、镇，[2013版数据](http://www.stats.gov.cn/tjsj/tjbz/tjyqhdmhcxhfdm/2013/index.html)。


## 拼音标注

### 拼音源
省市区这三级采用在线拼音工具转换，据说依据《新华字典》、《现代汉语词典》等规范性辞书校对，多音字地名大部分能正确拼音，`重庆`->`chong qing`，`朝阳`->`chao yang`，`郫都`->`pi du`，`闵行`->`min hang`，`康巴什`->`kang ba shi`。

镇级以下地名采用本地拼音库转换，准确度没有省市区的高。

### 拼音前缀
从完整拼音中提取的拼音前缀，取的是第一个字前两个字母和后两个字首字母，意图是让第一个字相同名称的尽量能排序在一起。排序1：`黑龙江helj、湖北hub、湖南hun`；排序2：`湖北hb、黑龙江hlj、湖南hn`，排序一胜出。


## 数据问题

1. id编号和国家统计局的编号基本一致，方便以后更新，有很多网站接口数据中城市编号是和这个基本是一致的。

2. id重复项目前是没有（已优化过了），不过以前采集（2013版）后直接对统计局的编号进行简单缩短后会有重复现象（算是精度丢失）。

3. 地区名字是直接去掉常见的后缀进行精简的，如直接清除结尾的`市|区|县|街道办事处|XX族自治X`，数量较少并且移除会导致部分名字产生歧义的后缀并未精简。

4. 2017版开始数据结尾添加了自定义编号的`港澳台` `90`、`海外` `91`数据，此编号并非标准编码，而是整理和参考标准编码规则自定义的，方便用户统一使用（注：民政部的台港澳编码为71、81、82）。

5. 参考链接：[统计用区划代码和城乡划分代码编制规则](http://www.stats.gov.cn/tjsj/tjbz/200911/t20091125_8667.html)，[民政部发布的行政区划代码](http://www.mca.gov.cn/article/sj/xzqh/)。


### 2019修正数据

- [issues/2](https://github.com/xiangyuecn/AreaCity-JsSpider-StatsGov/issues/2) `乐亭县` 的 `乐`读`lào` ，此县下面的`乐亭`读音均已修正。



## 使用js自行采集

在低版本chrome控制台内运行1、2、3打头的文件即可完成采集，前提是指定网页打开的控制台。这三个文件按顺序执行。

最新采集代码内对拼音转换的接口变化蛮大，由于优秀的那个公网接口采取了IP限制措施，就算使用了全自动的切换代理，全量转换还是极为缓慢，因此采用了本地转换接口和公网转换接口结合的办法，省市区三级采用公网接口，其他的采用本地接口。公网接口转换的正确度极高，本地的略差那么一点。

### 步骤1

1. 打开国家统计局任页面 http://www.stats.gov.cn/tjsj/tjbz/tjyqhdmhcxhfdm/。
2. 控制台内粘贴`1_抓取国家统计局城市信息.js`代码执行。
3. 采集完成自动弹出下载，保存得到文件`data.txt`。

### 步骤2

1. 打开拼音接口页面，具体看`2_抓取拼音.js`开头注释。
2. 复制`data.txt`内容到控制台执行，数据完成导入。
3. 执行`2_抓取拼音.js`内代码。
4. 拼音采集完成自动弹出下载，保存得到文件`data-pinyin.txt`。

注：如果是`2_x_抓取拼音.js`，依次同样的运行。

### 步骤3

1. 任意页面，最好是第二步这个页面。
2. 复制`data-pinyin.txt`内容到控制台执行，数据完成导入。
3. 执行`3_格式化.js`内代码。
4. 格式化完成自动弹出下载，保存得到最终文件`ok_data.csv`。




# :star:捐赠
如果这个库有帮助到您，请 Star 一下。

你也可以选择使用支付宝给我捐赠：

![](https://github.com/xiangyuecn/Recorder/raw/master/.assets/donate-alipay.png)  ![](https://github.com/xiangyuecn/Recorder/raw/master/.assets/donate-weixin.png)
