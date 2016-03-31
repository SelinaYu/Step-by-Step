# gulp学习笔记

标签（空格分隔）： 自动化开发工具

---
**点击这里查看**[gulp API文档][1]
<h3>gulp可以做的事情：</h3>

     1. 压缩CSS
     2. 压缩图片
     3. 编译Sass/LESS
     4. 编译CoffeeScript
     5. markdown转换html.....

**压缩这些的好处：**
降低文件大小，提高访问速度

gulp的使用步骤：
1. 安装gulp，跳转到指定目录，输入命令行：

      npm install -g gulp 
2. 将gulp的所有配置代码写在`gulpfile.js`文件
3. 在`gulpfile.js`所在目录打开命令行输入 `gulp 任务名`运行任务

**在这里，遇到了一个小问题，执行命令一直提示下面这个**：![gulp安装][2]
后来搜索了发现，建议不要全局安装gulp，因为gulp会自动引入相关的支持包，这些包安装在/usr/local/lib/node_modules/下，不利于管理，易引起冲突,并且全局gulp升级后与此项目gulpfile.js代码不兼容。所以建议在项目中安装，输入如下命令安装：
`$npm install --save-dev gulp`


gulp常用的模块如下：

      压缩js：   gulp-uglify
      压缩css：  gulp-minify-css
      压缩图片： gulp-imagemin
      编译less： gulp-less
      编译sass:  gulp-ruby-sass或者gulp-sass

使用gulp构建一个项目的步骤：

     1. 在命令行输入 npm init（会依次要求补全项目信息，生成package.json）
     2. 安装gulp到项目 $npm install --save-dev gulp
     3. 安装依赖的模块
     
示例：压缩css的gulpfile.js文件的代码如下：
```
// 获取 gulp
var gulp = require('gulp')

// 获取 minify-css 模块（用于压缩 CSS）
var minifyCSS = require('gulp-minify-css')

// 压缩 css 文件
// 在命令行使用 gulp css 启动此任务
gulp.task('css', function () {
    // 1. 找到文件
    gulp.src('css/*.css')
    // 2. 压缩文件
        .pipe(minifyCSS())
    // 3. 另存为压缩文件
        .pipe(gulp.dest('dist/css'))
})

// 在命令行使用 gulp auto 启动此任务
gulp.task('auto', function () {
    // 监听文件修改，当文件被修改则执行 css 任务
    gulp.watch('css/*.css', ['css'])
});

// 使用 gulp.task('default') 定义默认任务
// 在命令行使用 gulp 启动 css 任务和 auto 任务
gulp.task('default', ['css', 'auto'])
```
  [1]: http://www.gulpjs.com.cn/docs/api/
  [2]: http://7xq2ky.com1.z0.glb.clouddn.com/gulp1.png
  