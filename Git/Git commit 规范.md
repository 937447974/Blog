在多人协作项目中，良好的 commit 风格如下所示

![](https://raw.githubusercontent.com/937447974/Blog/master/Resources/2018072501.png)

这里使用的是 commitlint。命令行效果如下

<p align="center">
  <img width="600" src="https://raw.githubusercontent.com/937447974/Blog/master/Resources/2018072502.svg">
</p>

# 1 Commit message 的格式

Commitlint 基于 Angular 的规范。很多工具也是基于此规范, 它的 message 格式如下:

```
// Header
<type>(scope): <subject>
// 空一行
Body
// 空一行
Footer
```

Header 是必需的，注意冒号后面有空格，Body 和 Footer 可以省略。

1. Header：Header部分只有一行，包括三个字段：type（必需）、scope（可选）和subject（必需）
	1. type：用于说明 commit 的类型，被指定在 commitlint.config.js 的 type-enum。
	2. scope: 用于说明 commit 的影响范围，可以省略。
	3. subject：subject 是 commit 目的的简短描述，不超过50个字符，且结尾不加句号（.）。
2. Body: body 部分是对本次 commit 的描述，可以分成多行。
3. Footer: footer 用于不兼容变动和关闭ISSUE。

# 2 Commitlint 安装

## 2.1 commitlint.config.js

git 根目录新建 commitlint.config.js 文件，添加如下代码。

```
module.exports = {
    extends: ['@commitlint/config-conventional'],
    rules: {
        'subject-case': [0, 'never'],
        'type-enum': [
            2,
            'always',
            [
                "docs",     // Adds or alters documentation. 仅仅修改了文档，比如README, CHANGELOG, CONTRIBUTE等等
                "chore",    // Other changes that don't modify src or test files. 改变构建流程、或者增加依赖库、工具等
                "feat",     // Adds a new feature. 新增feature
                "fix",      // Solves a bug. 修复bug
                "merge",    // Merge branch ? of ?.
                "perf",     // Improves performance. 优化相关，比如提升性能、体验
                "refactor", // Rewrites code without feature, performance or bug changes. 代码重构，没有加新功能或者修复bug
                "revert",    // Reverts a previous commit. 回滚到上一个版本                
                "style",    // Improves formatting, white-space. 仅仅修改了空格、格式缩进、都好等等，不改变代码逻辑                
                "test"     // Adds or modifies tests. 测试用例，包括单元测试、集成测试等                
            ]
        ],
    }
};
```

## 2.2 package.json

命令行进入项目根目录，执行 `npm init` 创建 package.json。

打开 package.json 粘贴如下代码。

```
"devDependencies": {
    "@commitlint/cli": "^7.0.0",
    "@commitlint/config-conventional": "^7.0.1",
    "husky": "^0.14.3"
},
"scripts": {
    "commitmsg": "commitlint -E GIT_PARAMS"
}
```

执行命令 `npm install --save-dev @commitlint/{cli,config-conventional} husky` 安装 commitlint 和 husky。

安装完毕后，打开 package.json 后如下所示。

```
{
    "name": "yjcocoa",
    "version": "8.3.0",
    "description": "YJ系列开源库",
    "author": "阳君",
    "license": "MIT",
    "homepage": "https://github.com/937447974/YJCocoa",
    "repository": {
        "type": "git",
        "url": "https://github.com/937447974/YJCocoa.git"
    },
    "bugs": {
        "url": "https://github.com/937447974/YJCocoa/issues"
    },
    "main": "commitlint.config.js",
    "devDependencies": {
        "@commitlint/cli": "^7.0.0",
        "@commitlint/config-conventional": "^7.0.1",
        "husky": "^0.14.3"
    },
    "scripts": {
        "commitmsg": "commitlint -E GIT_PARAMS",
        "commit": "commit"
    }
}
```

执行测试命令  `echo 'feat: 测试 commitlint' | commitlint`，验证 commitlint 是否起效。

将 

```
# Node
node_modules/
package-lock.json
```

添加到 .gitignore 后推送代码到服务器。

他人 pull 代码后，命令行进入项目根目录执行 npm install 即可使用 commitlint 提交规范。

&#160;

----------

# Appendix

## Related Documentation

[commitlint](http://marionebl.github.io/commitlint/#/)

[git commit 提交规范 & 规范校验](https://blog.csdn.net/y491887095/article/details/80594043)

[Git Commint规范](https://www.colabug.com/1744239.html)

## Copyright

CSDN：[http://blog.csdn.net/y550918116j](http://blog.csdn.net/y550918116j)

GitHub：[https://github.com/937447974](https://github.com/937447974)