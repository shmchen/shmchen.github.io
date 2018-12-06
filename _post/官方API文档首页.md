### 官方API文档首页:
- http://lbsyun.baidu.com/index.php?title=uri/api/web

- 调起网页版百度地图：

```
// 路径规划

http://api.map.baidu.com/direction?origin=latlng:{{我的位置}}|name:&destination=latlng:24.474705,118.122452|name:&mode=walking&region=西安&output=html&src=yourCompanyName|yourAppName

http://map.baidu.com/?latlng=24.474705,118.122452&title=@%22%E5%96%9C%E6%9D%A5%E7%99%BB%E9%85%92%E5%BA%97%22&content=&@&output=html
NSString *urlStr = [NSString stringWithFormat:@"http://api.map.baidu.com/marker?location=24.474705,118.122452&title=%@&content=%@&output=html&src=北京法意|法律公众端", self.name, self.address];

// 定位
NSString *urlStr = [NSString stringWithFormat:@"http://api.map.baidu.com/direction?origin=latlng:%f,%f|name:&destination=latlng:%f,%f|name:&mode=walking&region=西安&output=html&src=yourCompanyName|yourAppName", self.locationPt.latitude, self.locationPt.longitude, self.coordinate2D.latitude, self.coordinate2D.longitude];

```


- 调起百度地图客户端
```
baidumap://map/direction?origin={{我的位置}}&destination=latlng:%f,%f|name=目的地&mode=walking&coord_type=gcj02
```

- 判断是否安装了百度地图

```
if ([[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:@"baidumap://"]]) {
            
            NSString *urlString = [[NSString stringWithFormat:@"baidumap://map/direction?origin={{我的位置}}&destination=latlng:%f,%f|name=目的地&mode=walking&coord_type=gcj02",self.coordinate2D.latitude, self.coordinate2D.longitude] stringByAddingPercentEscapesUsingEncoding:NSUTF8StringEncoding];
            if ([[UIApplication sharedApplication] canOpenURL:[NSURL URLWithString:urlString]]) {
                [[UIApplication sharedApplication] openURL:[NSURL URLWithString:urlString]];
            }else {
                [SVProgressHUD showImage:nil status:@"无法跳转百度地图"];
            }
            
        }else {
            [self showWebMap];
            [SVProgressHUD showImage:nil status:@"未安装百度地图,为您跳转网页地图"];
        }
```

- 添加百度地图schme白名单
    + info.plist添加数组属性==LSApplicationQueriesSchemes==
    + item: ==baidumap==