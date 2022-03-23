eslint是做js的代码格式化的，但是扩展性很强，包括变量是否使用，是否要console.log。而prettier是做所有代码的格式化，并且只专注于格式化，范围更广。

配合vscode插件elint和prettier 实现保存文件时验证。



vscode的插件可以单独配置，单独起作用。但是如果项目根目录中有`.eslintrc.js`和`.prettierrc.js`文件，则以`.eslintrc.js`和`.prettierrc.js`的配置为标准，执行它们的配置。
 prettier的效果在eslint的效果之后，如果配置冲突了，则prettier会覆盖eslint的配置。



```bash
// .editorconfig
root = true

[*]
indent_size = 4

// .eslintrc.js
indent: ['error', 4]
```



**case**

```
// .eslintrc.js
module.exports = {
    env: {
        browser: true,
        es2021: true,
        node: true,
    },
    extends: 'eslint:recommended',
    parserOptions: {
        ecmaVersion: 'latest',
        sourceType: 'module',
    },
    rules: {
        quotes: 0,
        semi: 1,
        'no-console': 1,
        // 不能用多余的空格
        'no-multi-spaces': 2,
        // 一行结束后面不要有空格
        'no-trailing-spaces': 2,
        // 引号类型
        quotes: [
            2,
            'single',
            {
                avoidEscape: true,
                allowTemplateLiterals: true,
            },
        ],
        indent: ['error', 4],
    },
};

// .prettierrc.js
module.exports = {
    semi: true,
    singleQuote: true,
};


```

[VSCode Prettier配置](https://www.jianshu.com/p/61a30aa40486)
