# async函数和await函数

## async函数

1. async和await结合可以让异步代码像同步代码一样
2. async函数的返回值为`promise对象`
3. promise对象的结果由async函数执行的返回值决定

```javascript
async function fn(){
  
  //return 'spl';
  //返回的结果不是一个Promise类型的对象，则返回的结果是一个成功的promise
  throw new Error("出错啦！！")
  //抛出错误，返回的结果是一个失败的Promise
  return new Promise((resolve,reject)=>{
    resolve('成功的数据');
  });
  //返回的结果是一个Promise对象，成功和失败与promise返回的值有关
}
const result = fn();
console.log(result);
```

## await函数

1. await 必须写在async函数中
2. await右侧的表达式一般为promise对象
3. await返回的是promise成功的值
4. await的promise失败了，会抛出异常，需要通过`try...catch`捕获处理

```javascript
//1.await 必须写在async函数中 2. 表达式一般是promise对象 3.返回的是成功的值
const p = new Promise((resolve,reject)=>{
  resolve("用户数据");
})
async function main(){
  let result = await p;
  console.log(result)
};
main();

//4.如果失败了，会抛出异常，需要用try…catch捕获错误信息
const p = new Promise((resolve,reject)=>{
  reject("失败了！！");
})
async function main(){
  try{
    let result = await p;
    console.log(result);
  } catch(e){
    console.log(e);
  }
};
main();
```

- 读取文件的实例

```javascript
const fs = require('fs');

function readWeiXue(){
  return new Promise((resolve,reject)=>{
    fs.readFile('<文件路径>',(err,data)=>{
      //如果失败
      if (err) reject(err);
      //如果成功
      resolve(data);
    })
  })
}

function readGuanShu(){
  return new Promise((resolve,reject)=>{
    fs.readFile('<文件路径>',(err,data)=>{
      //如果失败
      if (err) reject(err);
      //如果成功
      resolve(data);
    })
  })
}

function readChaYangShi(){
  return new Promise((resolve,reject)=>{
    fs.readFile('<文件路径>',(err,data)=>{
      //如果失败
      if (err) reject(err);
      //如果成功
      resolve(data);
    })
  })
}

//声明一个async函数

async function main(){
  //获取为学内容
  let weixue = await readWeiXue();
  let changyang = await readChayang();
  let guanshu = await readGuanshu();

  console.log(weixue.toString());
  console.log(chayang.toString());
  console.log(guanshu.toString());
}

main();
//发送AJAX请求，返回的结果是Promise对象
function sendAJAX(url){
  //1. 创建对象
  const x = new XMLHttpRequest();

  //2. 初始化
  x.open('GET',url)

  //3. 发送
  x.send();

  //4. 事件绑定
  x.onreadystatechange = function(){
    if (x.readyState===4){
      if (x.status >=200 && x.status <300){
        //成功
        reslove(x.response);
      }else{
        reject (x.status);
      }
    }
  }
}

//测试
const result = sendAJAX("https://api.apiopen.top/getJoke")
console.log(result);//返回的确实是一个promise对象
//转化为
const result = sendAJAX("https://api.apiopen.top/getJoke").then(value=>{
  console.log(value)
},reason()=>{})

//async 与 await 测试
async function main(){
  //发送AJAX请求
  let result = await sendAJAX("https://api.apiopen.top/getJoke");
  console.log(result);
}
```