1. xhr.send()  明明发送的是查询字符串，为什么formData对象可以传进去
```javascript
    // 4. 创建 FormData 对象
     var fd = new FormData()
     // 5. 向 FormData 中追加文件
     fd.append('avatar', files[0])
    // 6. 创建 xhr 对象
     var xhr = new XMLHttpRequest()
     // 7. 调用 open 函数，指定请求类型与URL地址。其中，请求类型必须为 POST
      xhr.open('POST', 'http://www.liulongbin.top:3006/api/upload/avatar')
      // 8. 发起请求
       xhr.send(fd)
```
2. 为什么实例化对象打印出来会有原型，它的原型不是和构造函数一样吗，在_proto_里
3. 同源策略不能获取不同源的地址，为什么案例中自己试验却可以调用给的接口呢
4. js对象本身是一个缓存对象？