**步骤1**package.json添加脚本命令
```
"scripts": {
 "build-prod": "vue-cli-service build --mode production",
 "build-dev": "vue-cli-service build --mode dev",
 "build-alpha": "vue-cli-service build --mode alpha",
 "build-pro": "vue-cli-service build --mode pro",
}
```
*****

**步骤2** 再项目根目录添加".env.alpha     .env.pro     "

.env.alpha内容是
`NODE_ENV='alpha'`
.env.pro的内容是
`NODE_ENV='pro'`
*****

**步骤3** 添加'setBaseUrl.js来设置url'
```
let baseUrl: string = "";   //这里是一个默认的url，可以没有
switch (process.env.NODE_ENV) {
    case 'development':
        baseUrl = "http://localhost:57156/"  //这里是本地的请求url
        break
    case 'alpha':   // 注意这里的名字要和步骤二中设置的环境名字对应起来
        baseUrl = "http://www.cnblogs.com/XHappyness/"  //这里是测试环境中的url
        break
    case 'production':
        baseUrl = "https://www.cnblogs.com/XHappyness/p/7686267.html"   //生产环境url
        break
}

export default baseUrl;

```
*****
** *当执行脚本 `npm run build-prod  npm run  build-alpha` 时可以看到 baseUrl是不一样的* **