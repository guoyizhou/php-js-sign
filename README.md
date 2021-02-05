# php-js-sign
php+js 前后端分离之签名
```php
    /**
     * 获取签名
     * @param $data 提交的数据
     * @param $key  安全密钥
     * @return bool
     * 前端参考签名 https://www.cnblogs.com/dcb3688/p/4608008.html
     */
    function getSign($request,$key='ABC123')
    {
        if(isset($request['sign'])) unset($request['sign']);
        ksort($request);
        $requestString = '';
        foreach($request as $k => $v) {
            if(is_array($v)) {
                continue;
            }
            $requestString .= "&".$k . '=' . urldecode($v);
        }
        return hash_hmac("md5",strtolower($requestString) , $key);
    }
```

```js
<script src="https://cdn.bootcdn.net/ajax/libs/blueimp-md5/2.18.0/js/md5.min.js"></script>
<script>
    var postData = {"demo":"hello"};
    /**
     * json 排序
     * 先排序再toLower,所以Did 在appid 之前
     */
    function jsonSort(jsonObj,sign_key='ABC123') {
        let arr = [];
        for (var key in jsonObj) {
            if(key != 'sign') {
                arr.push(key)
            }
        }
        arr.sort();
        let str = '';
        for (var i in arr) {
            //判定对象或数组不参数校验
            if(Array.isArray(jsonObj[arr[i]])) {
                continue;
            }
            str +="&"+arr[i].toLowerCase()+"="+decodeURI(jsonObj[arr[i]]).toLowerCase();
        }
        console.log(str);
        return md5(str,sign_key)
    }
    var str = jsonSort(postData);
    console.log(str);

</script>
```
