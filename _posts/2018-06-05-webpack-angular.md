---
layout: post
title: 使用webpack打包AngularJS1.X方法总结
date: 2018-06-05
categories: blog
tags: [webpack,angular]
description: 纯技术分享。
---
### webpack.config.js打包声明

```
var webpack = require('webpack'),
    HtmlWebpackPlugin = require('html-webpack-plugin'), //模板插件
    ExtractTextPlugin = require("extract-text-webpack-plugin"), //css分割插件
    // CleanWebpackPlugin = require('clean-webpack-plugin'), // 删除插件
    // NgAnnotatePlugin = require('ng-annotate-webpack-plugin'), //自动注入注解插件
    // autoprefixer = require('autoprefixer'),
    path = require('path'),
    buildPath = path.resolve(__dirname, ""), //发布目录
    __DEV__ = process.env.NODE_ENV === 'dev', //发布环境
    publicPath = '', //资源引用统一前缀
   // devtool = '', //source-map模式

    plugins = [
        new HtmlWebpackPlugin({

            template: __dirname + '/index1.html',
            filename: './public.html'
        }),

    ];

if (!__DEV__) {
    //压缩
    plugins.push(new webpack.optimize.UglifyJsPlugin({
        compress: {
            warnings: false

        },
        sourceMap: false,
        mangle: false
    }));

    publicPath = process.env.NODE_ENV == 'test' ? 'test.domain.com' : '';
    //devtool = 'source-map';
}

var config = {
    //入口文件配置
    entry: {
        app: path.resolve(__dirname, './entry.js'),

    },
    //文件导出的配置
    output: {
        path: buildPath,
        filename: "dist/[name].[hash:6].1.0.js",
        publicPath: publicPath,

    },
    //本地服务器配置
    devServer: {
        historyApiFallback: true,
        hot: true,
        inline: true,
        grogress: true，
        contentBase:"./"  //本地调试配置，目录
    },
    //模块配置
    module: {
        loaders: [
            // {
            //     test: /\.html$/,
            //     loader: 'html-withimg-loader'
            // },
            {
                test: /\.(png|jpg|gif)$/,
                loader: 'url-loader?limit=8192,name=img/[name].[hash:6].[ext]'
            }, {
                test: /\.(woff|woff2|eot|ttf|svg)(\?.*$|$)/,
                loader: 'url-loader?importLoaders=1&limit=1000&name=fonts/[name].[ext]'
            },
            {
                test: /\.css$/, loader: 'style-loader!css-loader'
            } //2.0貌似不支持缩写了
            // {
            //     test: /\.css$/,
            //     loaders: ['style', 'css'],
            // }, {
            //     test: /\.less$/,
            //     loader: 'style-loader!css-loader!less-loader'
            // },
        ]
    },
    // postcss: function() {
    //     return [autoprefixer()];
    // },
    //插件配置
   plugins: plugins,
    //调试配置
   // devtool: devtool
}

module.exports = config;

```
### 入口配置文件entry.js
##### 划重点！！！！
1. app.js声明文件必须放在index1.html即模板文件中引用；
2. 生成的app.xxxx.js即混淆打包后的js代码必须在index1模板页面底部引用（因为核心的jquery和angular在页面较靠上的位置引用，js从上到下的顺序读取）
```
/**
 * 创建者：陈凯歌   日期：2017-10-19
 * 说明：webpack入口文件
 * 部门：研发中心
 */
//CSS引入--内部系统CSS部分无法打包
require('./css/bootstrap.css');
require('./css/animate.css');
require('./vendor/modules/font-awesome-4.7.0/css/font-awesome.min.css');
require('./css/font.css');
require('./css/app.css');
require('./css/DC_Main.css');
require('./css/DC_Style.css');
require('./css/DC_NewStyle.css');
require('./vendor/modules/FixedColumns/bootstrap-table.css');
require('./vendor/modules/FixedColumns/bootstrap-table-fixed-columns.css');
require('./vendor/modules/FixedColumns/docs.min.css');
require('./vendor/modules/bootstrap-datetimepicker/bootstrap-datetimepicker.min.css');
require('./vendor/modules/jQuery-timedrag/css/layout.css');
require('./css/DCElse.css');
require('./vendor/select2/select2.css');
require('./vendor/select2/select2-bootstrap.css');
require('./vendor/modules/bootstrap-fileinput-master/bootstrap-fileinput-master/css/fileinput.min.css');
require('./vendor/modules/bootstrap-fileinput-master/bootstrap-fileinput-master/themes/explorer/theme.css');
require('./vendor/modules/zTree_v3/css/metroStyle/metroStyle.css');
require('./css/simple-line-icons.css');
//JS引入----------------------------------------------
require('./js/config.js');
require('./js/config.lazyload.js');
require('./js/config1.router.js');
require('./js/main.js');
require('./js/handop/self.js');
require('./js/factory/Common.js');
require('./js/services/Service.js');
require('./js/services/ui-load.js');
require('./js/filters/filter.js');
require('./js/filters/fromNow.js');
require('./js/directives/setnganimate.js');
require('./js/directives/ui-butterbar.js');
require('./js/directives/ui-focus.js');
require('./js/directives/ui-fullscreen.js');
require('./js/directives/ui-jq.js');
require('./js/directives/ui-module.js');
require('./js/directives/ui-nav.js');
require('./js/directives/ui-scroll.js');
require('./js/directives/ui-shift.js');
require('./js/directives/ui-toggleclass.js');
require('./js/directives/ui-validate.js');
require('./js/controllers/bootstrap.js');
require('./js/directives/repeatDone.js');
require("./js/directives/bsTableDirective.js");
require("./vendor/select2/select2Directive");//select2指令
require('./js/controllers/BackMaintenance/DepartmentManagementController.js');
require('./js/controllers/BackMaintenance/ModuleManagementController.js');
require('./js/controllers/BackMaintenance/PersonnelManagementController.js');
require('./js/controllers/BackMaintenance/PostManagementController.js');
require('./js/controllers/BackMaintenance/PostModuleManagementController.js');
require('./js/controllers/Financial/BasicControlTemplateController.js');
require('./js/controllers/Financial/BigEventExampleController.js');
require('./js/controllers/Financial/BondManagementController.js');
require('./js/controllers/Financial/BootstrapUploadTemplateController.js');
require('./js/controllers/Financial/ControlTemplatePageController.js');
require('./js/controllers/Financial/costAllocationController.js');
require('./js/controllers/Financial/financialManagementController.js');
require('./js/controllers/Financial/financialManagementSoftwareController.js');
require('./js/controllers/Financial/FixedColumnsTableExampleController.js');
require('./js/controllers/Financial/GetBondRecordController.js');
require('./js/controllers/Financial/ScrollNavTemplateController.js');
require('./js/controllers/Financial/TimeLineExampleController.js');
require('./js/controllers/Financial/totalFinancialManagementController.js');
require('./js/controllers/Financial/ZTreeExampleController.js');
require('./js/controllers/Login/LoginInController.js');
require('./js/controllers/Login/ModifyPasswordController.js');
require('./js/controllers/Login/PasswordRecController.js');
require('./js/controllers/Login/SignUpController.js');
require('./js/controllers/Operate/BaseInfoController.js');
require('./js/controllers/Operate/BaseMaintentController.js');
require('./js/controllers/Operate/BiddingInformationController.js');
require('./js/controllers/Operate/BiddingInformationZjController.js');
require('./js/controllers/Operate/contractManagementController.js');
require('./js/controllers/Operate/customerInformationManagementController.js');
require('./js/controllers/Operate/tenderManagementController.js');
require('./js/controllers/Personnel/MessageSendController.js');
require('./js/controllers/Personnel/PersonDetailController.js');
require('./js/controllers/Personnel/PersonFilesEditController.js');
require('./js/controllers/Personnel/PersonnelAttendanceController.js');
require('./js/controllers/Personnel/PersonnelFilesController.js');
require('./js/controllers/Personnel/PersonnelInformationController.js');
require('./js/controllers/Project/ExpenseController.js');
require('./js/controllers/Project/GatheringController.js');
require('./js/controllers/Project/GetReceiptListController.js');
require('./js/controllers/Project/MarginController.js');
require('./js/controllers/Project/ProjectArchiveController.js');
require('./js/controllers/Project/ProjectInfoController.js');
require('./js/controllers/Project/ProjectProductionDivController.js');
require('./js/controllers/Project/ProjectRevenueMagController.js');
require('./js/controllers/Project/ProjectScheduleMagController.js');
require('./js/controllers/Project/ProjectViewInfoController.js');
//--新增部分ctrl
require('./js/controllers/BackMaintenance/RoleModulePubController.js');
require('./js/controllers/BackMaintenance/RolePubController.js');
require('./js/controllers/BackMaintenance/ModulePubController.js');
require('./js/controllers/LeaderSite/LeaderWeeklyController.js');
require('./js/controllers/TaskDistribute/TaskDistributeController.js');
require('./js/controllers/Login/browerCtrl.js');
require('./js/controllers/BackMaintenance/PersonCorrelationController.js');
require('./js/controllers/Personnel/PersonLeaveController.js');
require('./js/controllers/Personnel/SelfInformationController.js');
require('./js/controllers/Financial/GatheringManagementController.js');
require('./js/controllers/Financial/financialGetReceiptController.js');
require('./js/controllers/Financial/financialGatheringController.js');
require('./js/controllers/Project/ExternalDetailController.js');
require('./js/controllers/Project/ExternalExpenseController.js');
require('./js/controllers/Project/ReceiptDivideController.js');
require('./js/controllers/Project/StatisticsController.js');
require('./js/controllers/Project/OverViewController.js');
require('./js/controllers/Project/ProResultArcCtrl.js');
require('./js/controllers/Project/CheckGatheringController.js');
require('./js/controllers/Project/OutputValueController.js');
require('./js/controllers/Project/ActualGatheringController.js');
require('./js/controllers/Project/ContractGatheringController.js');
require('./js/controllers/PreView/PreViewController.js');
```
### webpack打包必不可少的json文件

```
{
  "name": "hzz",
  "version": "1.0.0",
  "description": "",
  "main": "entry.js",
  "dependencies": {
    "webpack": "^2.6.1"
  },
  "devDependencies": {
    "autoprefixer": "^6.6.0",
    "clean-webpack-plugin": "^0.1.14",
    "css-loader": "^0.26.1",
    "extract-text-webpack-plugin": "^1.0.1",
    "file-loader": "^0.9.0",
    "html-webpack-plugin": "^2.24.1",
    "ng-annotate-webpack-plugin": "^0.1.3",
    "node-sass": "^4.1.1",
    "postcss-loader": "^1.2.1",
    "raw-loader": "^0.5.1",
    "sass-loader": "^4.1.1",
    "style-loader": "^0.13.1",
    "url-loader": "^0.5.7",
    "webpack": "^1.14.0",
    "webpack-dev-server": "^1.16.2"
  },
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"，
    //配合cmd本地调试命令
    "start": "webpack",
    "server": "webpack-dev-server --open"
  },
  "keywords": [],
  "author": "",
  "license": "ISC"
}

```
### 对angular路由的改造

```
.state('app.Control_Template_Page', {
                  url: '/Control_Template_Page',
                  templateUrl: 'view/Financial/Control_Template_Page.html',
                  resolve: {
                      deps: ['$ocLazyLoad',
                      function ($ocLazyLoad) {
                          //return $ocLazyLoad.load(['js/controllers/Financial/ControlTemplatePageController.js']);
                          //返回的ctrl要注释掉
                      }]
                  }
              })
```
## 打包时的npm操作步骤
1. 安装node.js
2. cmd命令，输入npm install webpack -g 。
3. cmd命令，根目录输入npm install 下载打包依赖。
4. cmd命令,根项目输入webpack进行JS、CSS、图片文件的打包处理。
5. 服务器部署更新时，更新dist文件夹的app.xxxx.js文件及根目录下的public.html文件。
## 本地调试方法，构建webpack本地服务器
1.  npm install anywhere -g来安装anywhere   server服务，确保你已经安装了最新的node.js！
2. anywhere 服务跑起来！啪啪啪飞起！注意，地址不是index.html而是public.html的前缀












