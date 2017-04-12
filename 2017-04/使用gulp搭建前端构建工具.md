# 使用gulp搭建前端构建工具
4/12/2017 10:06:33 PM 

----------
### Why？
今天跟大家分享的是关于目前自己的公司的项目优化中所使用到的gulp构建工具。目前公司对产品的优化决定采用VueJs来进行单页面开发，无疑这种新的技术框架对一个项目的效率会提升得更快更强。如今前端的发展速度飞快，仿佛稍微停止学习的脚步就会被时代抛弃，但我们也切忌过于急躁，所谓万卷不离其宗，打好基础是关键。  
### What？
在进行基础巩固的基础上，使用gulp构建工具可以在开发的过程中进行部分自动化构建，如对页面的监听，对样式的编译，对代码的压缩与打包。  
作为一个小型的项目来说，这些基本就足够了。  
搭建gulp最主要的是使用gulp的工作流，通过在gulp中的gulpfile文件编写我们的工作流，就仿佛一个个车间，在我们一条条命令下，就能完成我们需要的工作。  
### How？
##### 1.安装NodeJs
首先，最基本也最重要的是，我们需要搭建node环境。访问http://nodejs.org，然后点击大大的绿色的install按钮，下载完成后直接运行程序，就一切准备就绪。npm会随着安装包一起安装。  
为了确保Node已经正确安装，我们执行几个简单的命令。  
``` javascript
node -v
```  
回车（Enter），如果正确安装的话，你会看到所安装的Node的版本号，接下来看看npm。
``` javascript
npm -v
```  
这同样能得到npm的版本号。  
如果这两行命令没有得到返回，可能node就没有安装正确，尝试重启下命令行工具，如果还不行的话，只能回到第一步进行重装。
安装成功后我们就可以在目标目录中初始化环境  
``` javascript
npm init
```  
使用上述代码在项目空间中初始化环境，按步骤填写后会生成package.json文件
##### 2.安装gulp
使用npm工具下载gulp。在命令行中输入  
``` javascript
sudo npm install -g gulp 
```  
使用下面的命令查看gulp的版本号以确保gulp已经被正确安装。
``` javascript
gulp -v 
```
接下来，我们需要将gulp安装到项目本地  
``` javascript
npm install —-save-dev gulp
```  
这里，我们使用—-save-dev来更新package.json文件，更新devDependencies值，以表明项目需要依赖gulp。
Dependencies可以向其他参与项目的人指明项目在开发环境和生产环境中的node模块依懒关系，想要更加深入的了解它可以看看package.json文档。 
##### 3.gulpfile文档编写
在这里使用我在项目中使用的gulpfile文件。  
``` javascript
// 引入 gulp
var gulp = require('gulp');

// 引入组件
var jshint = require('gulp-jshint'); //语法检查
var sass = require('gulp-sass'); //sass编译
var concat = require('gulp-concat'); //合并
var uglify = require('gulp-uglify'); //js压缩
var rename = require('gulp-rename'); //重命名
var htmlmin = require('gulp-htmlmin'); //页面压缩
var minifyCss = require('gulp-minify-css'); //css压缩
var rev = require('gulp-rev'); //对文件名加MD5后缀
var revCollector = require('gulp-rev-collector'); //路径替换
var cheerio = require('gulp-cheerio'); //替换引用


// 默认任务 
gulp.task('default', ['sass', 'lint']);
// 开发环境gulp任务
gulp.task('watch', function() {
    // gulp.watch('./src/index.html', ['sass', 'lint']);
    gulp.watch('./src/scss/*.scss', ['sass']);
    gulp.watch('./src/js/*.js', ['lint']);
});
// 检查脚本 
gulp.task('lint', function() {
    gulp.src('./src/js/*.js')
        .pipe(jshint())
        .pipe(jshint.reporter('default'));
});
// 编译Sass 
gulp.task('sass', function() {
    gulp.src('./src/scss/*.scss')
        .pipe(sass())
        .pipe(gulp.dest('./src/css'));
});



// // 生产环境gulp任务
gulp.task('dev', ['cssConcat', 'scripts']);
// js合并，压缩文件 
gulp.task('scripts', function() {
    gulp.src('./src/js/*.js')
        .pipe(concat('all.js'))
        .pipe(gulp.dest('./dist/js'))
        .pipe(rename('all.min.js'))
        .pipe(uglify())
        .pipe(rev())
        .pipe(gulp.dest('./dist/js'))
        .pipe(rev.manifest({merge: true}))
        .pipe(gulp.dest('./rev/js/'));
});
// css合并压缩
gulp.task('cssConcat', function() {
    gulp.src('./src/css/**.css')
        .pipe(concat('style.min.css'))
        .pipe(minifyCss())
        .pipe(rev())
        .pipe(gulp.dest('./dist/css'))
        .pipe(rev.manifest({merge: true}))
        .pipe(gulp.dest('./rev/css/'));
});
//改变引用文件
gulp.task('rev', function() {
    gulp.src(['./rev/css/rev-manifest.json', './rev/js/rev-manifest.json', './dist/index.html'])
        .pipe(revCollector()) 
        .pipe(gulp.dest('./dist/'));
});
gulp.task('indexHtml', function() {
    return gulp.src('index.html')
        .pipe(cheerio(function ($) {
            $('script').remove();
            $('link').remove();
            $('body').append('<script src="./js/all.min.js"></script>');
            $('head').append('<link rel="stylesheet" href="./css/style.min.css">');
        }))
        .pipe(gulp.dest('dist/'));
});
//压缩html文件
gulp.task('minify', function() {
    return gulp.src('dist/*.html')
        .pipe(htmlmin({
            collapseWhitespace: true
        }))
        .pipe(gulp.dest('./dist/'));
});
```
基本上在这样的工作流程中，代码的压缩合并，监听发布都可以使用gulp流来控制。  
在使用gulpfile之前，要先对所需要的依赖进行安装。使用npm对依赖进行下载，这里下载的就是文件中所require引用的依赖，这里就不将全部写下来了。
``` javascript
npm i gulp-jshint --save-dev
```
gulp的工作流程十分简单，主要用task，run，watch，dest来进行任务搭建。

 - task主要作为任务的新建，即像我文件中所写的
 - run起运行任务的命令
 - watch起监听命令，作为对工程中文件的变化起监听作用，这一点如果再搭配页面重加载效果，即gulp-connect依赖，则可以实现页面无需手动刷新操作
 - dest主要是对文件的路径进行更改
 
更多详细的可以参考gulp的官网。熟悉了就赶紧尝试起来吧，趁年轻，大胆试错。  
### Next？
下一次的分享可能会收集在论坛上所了解到的一些我经常在项目中看见的代码，同时也是作为自己一个技术积累，与大家一起学习。CU tomorrow
### Finished
我的技术博客主要以github上创建的一个仓库为主，因为前端论坛太多了，我无从下手，所以一般都会先在github上整理完后再分发到各个社区，有兴趣请戳  
☛ [MolyCHNs Blogger](https://github.com/BendMoly/MolyCHNs-Blogger)