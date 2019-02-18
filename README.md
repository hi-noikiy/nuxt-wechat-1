# Nuxt实现微信网页授权前后端分离（demo）

该项目目的在于
* 展示微信网页授权、JSSDK功能的前后端分离。
* 采用单页（SPA）的开发模式，便于与当前的开发技术接轨（zhuangbi）。而且可以通过netlify这样的静态托管服务器来托管前端页面（甚至是调试），确实带来比较多便利。
* 展示如实使用优秀的wechat库（EasyWechat）来实现服务端
项目分为server+client。
* 本人为业余软件开发爱好者，第一次分享代码，如果有用还请star，帮忙改进代码。

## server
采用更快实现服务器，采用laravel，overtrue/laralve-wechat，overtrue/laralve-wechat-socialite实现后台功能

### 初始化项目
```bash
cd server
composer install
cp .env.example .env
```

### 然后编辑.env文档，配置一下配置项
```
WECHAT_OFFICIAL_ACCOUNT_APPID
WECHAT_OFFICIAL_ACCOUNT_SECRET
MOCK_WECHAT_OPENID
MOCK_WECHAT_NICKNAME
MOCK_WECHAT_HEADIMGURL
```

### 运行服务器：
```bash
php artisan serve
```

## client
采用nuxt.js 的vue.js通用框架，通过插件、中间件实现微信授权

### 初始化项目
```bash
cd client
yarn install
cp env.example.js env.js
```

### 然后编辑env.js文档，配置以下关键配置项
```
wechatAppid
appBaseUrl
apiBaseUrl
```
env.js取了NODE_ENV，实现production/development两种环境的自动切换。大大降低微信开发时需要采用生产服务器进行调试网页授权和jssdk的切换配置的麻烦。
* 当运行```yarn run dev```自动读取dev配置
* 当执行```yarn generate```自动读取prod配置

### 关于前端一些技术实现细节的说明
* 采用lscache包用于保存微信授权信息到localStorage中，失效时间可以在env.js中配置
* 微信授权共有【前端直接mock】、【服务器mock】、【真实mock】三种模式，第一种方便本地调试，第二种方面在服务器加入jwt，写数据等操作，更符合开发调试。
* wechatAuth插件目前state前端跳转前生产的随机字符串，可以保存到localStorage中回调后校验。目前没有启用，感觉没必要。

### nuxt中的微信授权中间件使用
参考pages/wechat.vue，核心代码为：
```js
  computed: {
    wechatUser() {
      return this.$store.state.wechat.user
    }
  },

  middleware: ['wechatAuth']
```


# 参考
[wkl007/vue-wechat-login](https://github.com/wkl007/vue-wechat-login/blob/master/src/plugins/wechatAuth.js)

其他用到的依赖就不在此一一致谢了