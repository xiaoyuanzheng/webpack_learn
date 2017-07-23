# webpack 01 入门指南
1. ## 使用webpack项目准备

    创建一个空白的文件夹，然后按一下步骤生成
    
2. ## 准备package.json文件
    package.json:此文件被npm用于存储项目的元数据，以便将此项目发布为npm模块。你可以在此文件中列出项目依赖的webpack插件，放置于devDependencies配置字段内。

    生成package.json的命令：npm init
    
3. ## 安装webpack
    安装命令：npm install -g webpack(全局安装)

    向已经存在package.json文件中添加webpack插件最简单的方式是通过npm install -save-dev命令。此命令不光安装了，还会自动将其添加到devDependencies配置段中，命令如下：
    npm install webpack --save-dev

4. ## webpack.config.js

    告诉webpack它需要做什么
    
    ```
    var webpack = require('webpack');
    var commonsPlugin = new webpack.optimize.CommonsChunkPlugin('common.js');
     
    module.exports = {
        //插件项
        plugins: [commonsPlugin],
        //页面入口文件配置
        entry: {
            index : './src/js/page/index.js'
        },
        //入口文件输出配置
        output: {
            path: 'dist/js/page',
            filename: '[name].js'
        },
        module: {
            //加载器配置
            //关键配置，告知webpack每一种文件用什么加载器来处理
            loaders: [
                { test: /\.css$/, loader: 'style-loader!css-loader' },
                { test: /\.js$/, loader: 'jsx-loader?harmony' },
                { test: /\.scss$/, loader: 'style!css!sass?sourceMap'},
                { test: /\.(png|jpg)$/, loader: 'url-loader?limit=8192'}
                //配置信息的参数“?limit=8192”表示将所有小于8kb的图片都转为base64形式（其实应该说超过8kb的才使用 url-loader 来映射到文件，否则转为data url形式）
            ]
        },
        //其它解决方案配置
        resolve: {
            root: 'E:/github/flux-example/src', //绝对路径
            extensions: ['', '.js', '.json', '.scss'],
            alias: {
                AppStore : 'js/stores/AppStores.js',
                ActionType : 'js/actions/ActionType.js',
                AppAction : 'js/actions/AppAction.js'
            }
        }
    };
    ```
    说明：
    1.  插件项
    
        CommonsChunkPlugin 的插件，它用于提取多个入口文件的公共脚本部分，然后生成一个 common.js 来方便多页面之间进行复用。
    
    2. entry&&output
        
        它是页面入口文件配置，output 是对应输出项配置（即入口文件最终要生成什么名字的文件、存放到哪里）
    
    3. module.loaders
    
        它是最关键的一块配置。它告知 webpack 每一种文件都需要使用什么加载器来处理
        
        如上，”-loader”其实是可以省略不写的，多个loader之间用“!”连接起来,注意所有的加载器都需要通过 npm 来加载.拿最后一个 url-loader 来说，它会将样式中引用到的图片转为模块来处理，使用该加载器需要先进行安装：npm install url-loader -save-dev
        
        
5. ## 练习
    1. 新建空白文件夹 lesson_01
    2. git命令行，准备package.json文件：npm install
    3. 安装webpack：npm -g webpack install or npm -g webpack install -save-dev
    4. 在lesson_01中新建index.html,index.js文件,如下
        
    ```
    //index.html
    <body>
    <script type="text/javascript" src="index.bundle.js"></script> 
    </body>
    
    
    //index.js
    document.write('hello');
    ```
    5. 将index.js文件转化为index.bundle.js，执行命令
    webpack index.js  index.bundle.js
