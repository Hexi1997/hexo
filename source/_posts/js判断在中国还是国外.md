---
title: JS判断在中国还是国外
date: 2021-07-17 09:02:07
type: tags
tags: [大前端, js]
categories: js
---

## 方案一：geolocation 原生 api

navigator.geolocation.getCurrentPosition

目前大多数现代浏览器均支持，但是获取位置信息需用户授权同意。

![image-20210714140551126](https://gitee.com/chen_hexi/image-source/raw/master/img/20210714143354.png)

用户点击允许之后刷新当前网页才可以获取到经纬度

![image-20210714140648718](https://gitee.com/chen_hexi/image-source/raw/master/img/20210714143357.png)

**结论**：<font color='red' size="20px">不可行</font>

## 方案二：高德地图 ip 定位

高德地图 webapi : ip 定位，引入高德地图定位 js，根据 ip 定位判断是否在中国。

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210714141631.png" alt="image-20210714141611698" style="zoom: 80%;" />

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210714143726.png" alt="image-20210714143013060" style="zoom:80%;" />

代码如下

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>ip定位</title>
  </head>
  <body>
    <script
      type="text/javascript"
      src="https://webapi.amap.com/maps?v=1.4.15&key=92a797e5c79c37cf76d5038e368b5d50"
    ></script>
    <script>
      AMap.plugin("AMap.CitySearch", function () {
        var citySearch = new AMap.CitySearch();
        citySearch.getLocalCity(
          function (status, result) {
            if (status === "complete" && result.info === "OK") {
              // 查询成功，result即为当前所在城市信息(国内)
              console.log(status, result);
            } else {
              //由于目前高德地图webapi只支持境内。通过vpn翻墙测试。国外用户访问此接口也是秒回。status为no_data  result为{}，代表用户在国外
              console.log(status, result);
            }
          },
          function (error) {
            console.error(error);
          }
        );
      });
    </script>
  </body>
</html>
```

成功返回的数据如下

![image-20210714143650899](https://gitee.com/chen_hexi/image-source/raw/master/img/20210714143732.png)

**缺点**：

- 需要单独引入高德地图 api 的 js 文件，300kb 左右。
- 因为 pc 设备上大都缺少 GPS 芯片，所以在 PC 上的定位主要通过 IP 定位，该服务的失败率在 5%左右（来自高德地图官方）
- 免费版每个 key 每日限额 30 万次，并发量上限 200 次。可使用多个 key，也可充钱提升配额

**结论**：<font color='green' size="20px">可行</font>

## 方案三：搜狐接口

`http://pv.sohu.com/cityjson?ie=utf-8`

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>ip定位</title>
  </head>
  <body>
    <script src="http://pv.sohu.com/cityjson?ie=utf-8"></script>
    <script>
      console.log(returnCitySN);
    </script>
  </body>
</html>
```

国外

![image-20210714144546165](https://gitee.com/chen_hexi/image-source/raw/master/img/20210714144619.png)

国内

![image-20210714144615028](https://gitee.com/chen_hexi/image-source/raw/master/img/20210714144621.png)

**可以根据 cid 进行判断国内还是国外**

**缺点**：

- 依赖第三方服务，可能不稳定

**结论**：<font color='green' size="20px">可行</font>

## 方案四： MaxMind 接口

https://dev.maxmind.com/geoip/geolocate-an-ip/client-side-javascript#3-call-an-api-method-and-provide-callbacks

第三方库：MaxMind 的 GeoIP®Databases and Services，他们有自己的 IP 库，提供各种准确的接口，付费的可以根据定位很准确，不付费的只可以模糊定位到国家，不过已经符合我们的需求。

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>

  <body>
    <div>目前所在：<span id="result"></span></div>
    <script
      src="//geoip-js.com/js/apis/geoip2/v2.1/geoip2.js"
      type="text/javascript"
    ></script>
    <script>
      var test = (function () {
        var onSuccess = function (geoipResponse) {
          /* There's no guarantee that a successful response object
           * has any particular property, so we need to code defensively. */
          if (!geoipResponse.country.iso_code) {
            return;
          }
          /* ISO country codes are in upper case. */
          var code = geoipResponse.country.iso_code.toLowerCase();
          document.getElementById("result").innerHTML = code;
        };
        var onError = function (error) {};
        return function () {
          geoip2.country(onSuccess, onError);
        };
      })();
      test();
    </script>
  </body>
</html>
```

![image-20210714151829252](https://gitee.com/chen_hexi/image-source/raw/master/img/20210714151832.png)

国内外访问速度都还可以

**缺点**：

- 依赖第三方服务，可能不稳定
- 引入 js，3kb 左右

**结论**：<font color='green' size="20px">可行</font>

## 方案五：访问一个 404 的网站

访问一个境内无法访问的网站，比如 google.com，无法访问则在国内，可以访问则在国外

## 方案六：自建服务

自建类似搜狐的 ip 定位接口。需要服务端配合（自建 ip 库之类）

**结论**：<font color='green' size="20px">可行</font>

## 方案七：通过浏览器语言判断 navigator.language

根据浏览器语言跳转不同的语言页面。

```js
function isInChain() {
  //兼容ie
  return (
    (navigator.language || navigator.browserLanguage).toLowerCase() === "zh-ch"
  );
}
```

**结论**：<font color='green' size="20px">可行</font>

## 方案八：使用 cloudflare 接口

https://www.cloudflare.com/cdn-cgi/trace，get接口，直接访问即可获取具体返回，根据loc判断即可

<img src="https://gitee.com/chen_hexi/image-source/raw/master/img/20210714171936.png" alt="image-20210714171932621" style="zoom:80%;" />

**缺点**：

- 依赖第三方服务，可能不稳定

**结论**：<font color='green' size="20px">可行</font>

## 总结

推荐使用方案八
