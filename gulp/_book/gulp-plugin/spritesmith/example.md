# 实例

## 例子1
```
gulp.task('sprites', function () {
    var spriteData = gulp.src(assetsPath + 'images/icon/*.png').
        pipe(spritesmith({
            imgName: 'icon.png',
            cssName: '_icon.scss',
            padding: 4,
            cssTemplate: './css.hbs',
            cssOpts: {
                functions: false,
                fileName: 'icon',
                version: new Date().getTime()
            },
        }));

    var imgStream = spriteData.img.pipe(gulp.dest(assetsPath + 'images/'));
    var cssStream = spriteData.css.pipe(gulp.dest(assetsPath + 'styles/'));

    // Return a merged stream to handle both `end` events
    return merge(imgStream, cssStream);
});
```
## 例子2（此例子Author[HelKyle](https://www.w3ctrain.com/2015/12/09/generating-sprites-with-gulp/)）
```
var configs = {
  //修改图片位置
  spritesSource: './src/image/sprites/*.*',
  spritesMithConfig: {
    //由于图片最终是要放到七牛上，这里的cssOpts用来当成最终scss文件中的变量名，详情看scss.template.mustache
    cssOpts: 'spriteSrc',
    imgName: 'sprite.png',
      cssName: 'sprite.scss',
      cssFormat: 'scss',
      cssTemplate: 'scss.template.mustache',
      algorithm: 'binary-tree',
      padding: 8,
      cssVarMap: function(sprite) {
        sprite.name = 'icon-' + sprite.name
      }
  },
  spritesOutputPath: './built/sprite/'
}
//总命令
gulp.task('sprite', function(callback) {
  runSequence(
    'sprite:build', 
    'sprite:images',
    'sprite:open',
    callback
  )
});
```
### 只可意会，不可言传