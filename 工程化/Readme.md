# webpack与glup

[webpack](https://juejin.cn/post/6844904031240863758#heading-6)

[webpack vs Glup](https://www.cnblogs.com/iovec/p/7921177.html)

## 用法
```javascript
// 流式
gulp.task('build:less', function(){
    return gulp.src('./src/*.less')
        .pipe(less())
        .pipe(gulp.dest('./dist'));
});
```

![image](https://user-images.githubusercontent.com/11763399/158203106-e3c5eefc-a832-401e-a4bc-d9ea1fbdab1f.png)
