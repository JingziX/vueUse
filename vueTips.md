## vue开发小总结

1.后台获取数据的方法
使用axios，作者微博上公告原来的vue-resource不再是官方推荐的ajax库


----------

2.router-link链接跳转
假设oid为参数，/a/b.html为配置的地址 `http://localhost/a/b.html`
dom上的写法为 

``` stylus
<router-link :to="{path : '/a/b',query:{id:oid}}"  class="item" tag="li"></router-link>
```
js的写法为

``` stylus
this.$router.push({ path: '/a/b', query: { id: oid }})
```

当前跳转的地址为
`http://localhost/a/b?id=oid `

获取当前id的方法
`var thisID = this.$route.query.id`

3.列表单选与列表多选的写法

 - 单选
 当前的index值与自己设置的默认值相同
 - 多选
 set值isCheck，判断是否为true

``` stylus
vue.$set(object,key,value);

if(typeof _self.olist.isCheck == "undefined"){
   this.$set(_self.olist,"isCheck",true)  
}else{
   _self.olist.isCheck=!_self.olist.isCheck;
}

```

4.filter--注册或获取全局过滤器（筛选数据）
放在computed中因为计算属性的结果会被缓存，结合第三点，筛选出全部选中的值，将筛选的值明明为newList，则进行如下操作

``` stylus
newList: function(){
    return this.olist.filter(function(oitem){
        if (oitem.isCheck) {
            return oitem;
        }
    })
}
```
当前得到的newList就是选中的值


5.合并两个对象

``` 
var detail={
	a:'',
	b:'',
	c:''
}
var output={
	a:'',
	b:''
}
for(var i in output){
    detail[i]=output[i];
}
enter code here
```
非常重要的一点，不要写成  detail = output，然后再重新赋值

6.正则特殊符号替换(正则的有另一篇总结文章)
``` 
.replace(/[\\\-*/]/g, "");
``` 



PART2. getCurrentPosition定位失败的原因有哪些?
1.IP精确定位失败，message包含‘Get ipLocation failed.’信息，精确IP定位服务无法完全覆盖所有IP和用户，故而失败；
2.sdk定位失败，请检查sdk的key是否设置好，以及webview的定位权限及应用和系统的定位权限是否开启。
3.浏览器定位失败，有多种情况：  
1. 第一种情况，浏览器不支持原生定位接口，如IE较低版本的浏览器等，message字段包含‘Browser not Support html5 geolocation.’信息；   
2. 第二种情况，用户禁用了定位权限，需要用户开启定位权限，message字段包含‘Geolocation permission denied.’   
3. 第三种情况，浏览器禁止了非安全域的定位请求，比如Chrome、IOS10已陆续禁止，这时候需要升级站点到HTTPS，message字段也是包含‘Geolocation permission denied.’信息。注意Chrome不会禁止localhost等域名HTTP协议下的定位；   
4. 第四种情况，浏览器定位超时，包括原生的超时，可以适当增加超时属性的设定值以减少这一现象，另外还有个别浏览器本身的定位接口就是黑洞，完全没有回应，也会超时返回失败，message字段包含‘Get geolocation time out.’信息；  
5. 第五种情况，确实定位失败，Chrome、火狐以及部分套壳浏览器接入的定位服务在国外，有较大限制，失败率高，message字段包含‘Get geolocation failed.’信息。



