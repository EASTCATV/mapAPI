#mapAPI


>最近两天在研究地图API,对比百度,腾讯地图说一下利弊

###总结一下

* 腾讯:不需要身份证也能注册,只需要企业真证就行


* Y的,百度就不行,非要实名认证身份证,咱们也知道,对于BAT实名的项目越多,你的互联网画像越清晰!!! 

* 结论:还是用腾讯的吧

#下面展示一下腾讯地图API

> 原始代码:git@github.com:EASTCATV/mapAPI.git





##demo
###初始化位置
```
var searchService,map,markers = [];
var init = function() {
    var center = new qq.maps.LatLng(39.936273,116.44004334);
    map = new qq.maps.Map(document.getElementById('container'),{
        center: center,
        zoom: 13
    });```
   
       //获取城市列表接口设置中心点
    citylocation = new qq.maps.CityService({
        complete : function(result){
            map.setCenter(result.detail.latLng);
        }
    });
    //调用searchLocalCity();方法    根据用户IP查询城市信息。
    citylocation.searchLocalCity();
    
    
    
       //绑定单击事件添加参数
    qq.maps.event.addListener(map, 'click', function(event) {
    	//清屏
    	
    	
    	
    	//点击时间调用方法:搜索
    	//search();
    	 //创建marker
    	 //点击创建标记
    /*	  var centers = new qq.maps.LatLng(event.latLng.getLat(),event.latLng.getLng());
    var marker = new qq.maps.Marker({
        position: centers,
        map: map
    });*/
    	
    	searchKeyword(event.latLng.getLat(),event.latLng.getLng());
        /*alert('您点击的位置为: [' + event.latLng.getLat() + ', ' +
        event.latLng.getLng() + ']');*/
       
       
       //设置圆形
    new qq.maps.Circle({
        center:new qq.maps.LatLng(event.latLng.getLat(),event.latLng.getLng()),
        radius: 2000,
        map: map
    });
       
       
       
       
    });
    
    
    
    
    
    var latlngBounds = new qq.maps.LatLngBounds();
    //调用Poi检索类
    searchService = new qq.maps.SearchService({
        complete : function(results){
            var pois = results.detail.pois;
            for(var i = 0,l = pois.length;i < l; i++){
                var poi = pois[i];
                latlngBounds.extend(poi.latLng);  
                var marker = new qq.maps.Marker({
                    map:map,
                    position: poi.latLng
                });

                marker.setTitle(i+1);
                
                markers.push(marker);
            }
            map.fitBounds(latlngBounds);
        }
    });