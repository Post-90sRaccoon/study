# Axios

* 通过 `json-server` 快速搭建 http 服务器

## CancelToken

```javascript
let cancel = null ;

if(cancel !== null) {
  cancel()
}
axios({
  method:'GET',
  url:'http://localhost:3000/posts',
  cancelToken: new axios.CancelToken(function executor(c){
    cancel = c 
  })
}).then(res=> {
  console.log(res)
  cancel = null 
})
```

