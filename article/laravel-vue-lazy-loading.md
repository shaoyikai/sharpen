### 需要安装的npm包

```npm install --save-dev babel-plugin-syntax-dynamic-import```

### 在项目根目录放置文件.babelrc

```json
{
  "plugins": ["syntax-dynamic-import"]
}
```

### webpack.mix.js 配置参考

```js
let mix = require('laravel-mix');
const path = require('path');

mix.webpackConfig({
    entry: {
        admin: "./resources/assets/js/admin/admin.js",
        app: "./resources/assets/js/app/app.js",
        vendor: ['vue', 'vue-router', 'axios', 'vuex', 'jquery', 'lodash']
    },
    output: {
        publicPath: '/',
        filename: 'js/[name].js',
        chunkFilename: 'js/children/[name].js'
    },
    module: {
        rules: [
            {
                test: /\.jsx?$/,
                exclude: /node_modules(?!\/foundation-sites)|bower_components/,
                use: [
                    {
                        loader: 'babel-loader',
                        options: Config.babel()
                    }
                ]
            }
        ]
    },
    resolve: {
        alias: {
            '@': path.resolve('resources/assets/sass')
        }
    }
})

mix.sass('resources/assets/sass/app.scss', 'public/css');
mix.sass('resources/assets/sass/admin.scss', 'public/css');

```

### vue 路由配置参考

```js
import Vue from 'vue'
import Router from 'vue-router'

const Hello = () => import("./components/hello")

Vue.use(Router)

export default new Router({
    mode: 'history',
    routes: [
        {path: '/hello', component: Hello},
    ]
})
```