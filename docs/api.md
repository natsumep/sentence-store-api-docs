
## 接口的来源 
有过一段时间，每天早上给女朋友来一句 `彩虹屁🌈` 成为了惯例;  
后来，我又做个一个两人的 个人的私密空间;     
在里面这个 `彩虹屁🌈` 又排上了用场;    
再然后，我已经不满足于`彩虹屁🌈 ` 了;  
我会在网上收纳一些句子。他不仅仅是彩虹屁;    
可能是某个动漫的热血句子、可能是一句诗、可能是一句歌词;  
我把这些都收纳到数据库，通过随机的获取方式，放到了私密空间，放到了博客;  
在某个不经意间。看到某条之前自己收藏的句子。还有一点点小惊喜;     
我想，像我这样的，应该有很多吧。可能这个中二的同学刚好是一个程序猿（媛）;   
可能又刚好像我一样，或许刚好就和我一样喜欢一些这样的小惊喜;  
于是，我把这些做成了开放调用的 api 接口;  
如果刚好你需要，可以用起来~


> 在一次不经意间的 `F12` 我看到了一个有着共同想法的项目 [一言（Hitokoto）](https://hitokoto.cn/) 这里还有很多句子是他们这里收纳来的呢~

## 碎碎念
- 后端采用了 `Node.js` + `mysql` + `pm2` + `nginx` 的方式开发部署。
- 目前就我一个人开发，云服务器资源有限，但是我尽可能的保证服务的稳定
- 建议做好容错处理，避免因为服务器宕机导致页面显示异常。
- 希望大家能有友好的使用接口，避免大量的调用。
- 目前单个 `ip` 每秒 `QPS` 限制为 `10 QPS` (每秒限制 `10` 次访问)。

## 接口介绍
目前有两类接口，分别是彩虹屁🌈 、和句子接口。  
1. 彩虹屁🌈 接口仅支持随机查询一条彩虹屁。
2. 句子接口支持分类查询，可以同时包含多种类型，目前也是从中随机查一条，后续将会支持从自己收藏中查询。

## 接口公共信息
1. 返回参数
接口统一返回 `json` 类型参数。如果需要特殊类型，可以留言区提需求哟~

```json
{
    "body":{
       // ... more
    },
    "msg":"success",
    "status":200
}

```
### 响应参数说明

|参数名称|说明|类型|
|-|-|-|
|body|查询的详细内容，具体参考下方接口|object|
|status|接口响应状态码，通常都是 200（成功）非200 都是异常|number / integer(int32)|
|msg|接口响应描述，200成功响应 success|string|

### 接口响应状态码

|状态码|说明|
|-|-|
|200|成功，通常状态下都是200|
|500|服务器内部处理异常，|
|600|查询失败，一般出现在已知错误的情况下，出现原因会在msg中给出，可以用来提示用户|

## 公共响应状态

## 接口地址
## 1. 彩虹屁接口🌈  

> 接口地址：https://api.tinker.run/caihong GET [点我访问](https://api.tinker.run/caihong)

!> 彩虹屁接口目前暂时没有参数过滤。默认随机返回一条数据

### 响应格式

```json
{
    "body":{
        "id":146,
        "content":"人生百味，唯你是甜。",
        "likes":0,
        "views":23,
        "viewsToday":3,
        "isCollect":false,
        "isLike":false,
        "collects":0
    },
    "msg":"success",
    "status":200
}
```

### 响应内容说明
 
|参数|说明|类型|
|-|-|-|
|id| 当前彩虹屁id，后续用于点赞收藏 | number / integer(int32)|
|content| 具体彩虹屁的内容 |string|
|likes| 共有个多少喜欢数。单个登录用户 or 单个ip，一天仅能对一条点一次喜欢|number / integer(int32)|
|views| 类型曝光数，查询一次+1 ；不包含今日 | number / integer(int32) |
|viewsToday|今日曝光数|number / integer(int32)|
|collects|累计收藏量，需登录用户才可以收藏|number / integer(int32)|
|isLike|未登录默认是false;登录用户是否当日点过喜欢，ps: 未处理未登录用户是否当日点过喜欢|boolean|
|isCollect|未登录默认是false;登录用户是否已收藏|boolean|


## 2. 句子接口

> 接口地址：https://api.tinker.run/sentence GET [点我访问](https://api.tinker.run/sentence)

### 查询参数

|参数名称|参数说明|请求类型|是否必须|数据类型|
|-|-|-|-|-|
|t|根据分类查询,支持多选,具体分类见下方，不传默认为全部随机|path|false|string|

```js
// 从 诗词、歌词中随机查询
fetch("https://api.tinker.run/sentence?t=1,11") 
.then(response => response.json())
.then(data=>{
    // console.log(data)
    /**
    {
        "body":{
            "id":2061,
            "content":"有三秋桂子，十里荷花。",
            "likes":2,
            "views":49,
            "viewsToday":1,
            "contentLength":11,
            "source":"望海潮·东南形胜",
            "type":"诗词",
            "collects":0,
            "author":"柳永",
            "isCollect":false,
            "isLike":false
        },
        "msg":"success",
        "status":200
    }
    */
})
```

### 句子类型

|类型|说明|
|-|-|
|1|原创|
|2|诗词|
|3|影视|
|4|动画|
|5|漫画|
|6|游戏|
|7|文学|
|8|网络|
|9|网易云|
|10|哲学|
|11|歌词|
|100|其他|

!> 多个分类查询 ?t=1,2,3  英文分号分割

### 响应参数
|参数|说明|类型|
|-|-|-|
|id| 当前句子id，后续用于点赞收藏 | number / integer(int32)|
|content| 具体句子的内容 |string|
|contentLength|内容长度|number / integer(int32)|
|source|句子的出处， 可能为空字符串| string  |
|author|句子的作者， 可能为空字符串| string  |
|type|句子的分类 中文名|string|
|likes| 共有个多少喜欢数。单个登录用户 or 单个ip，一天仅能对一条点一次喜欢|number / integer(int32)|
|views| 类型曝光数，查询一次+1 ；不包含今日 | number / integer(int32) |
|viewsToday|今日曝光数|number / integer(int32)|
|collects|累计收藏量，需登录用户才可以收藏|number / integer(int32)|
|isLike|未登录默认是false;登录用户是否当日点过喜欢，ps: 未处理未登录用户是否当日点过喜欢|boolean|
|isCollect|未登录默认是false;登录用户是否已收藏|boolean|

