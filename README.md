
# 实现git commitlint规范化

+ node版本：12.18.4

1. 首先在命令行执行以下命令：

    ```bash
        npm install husky -D
        npm install @commitlint/config-conventional @commitlint/cli -D
    ```

2. 生成commitlint配置文件：

    ```bash
        echo "module.exports = {extends: ['@commitlint/config-conventional']};" > commitlint.config.js
    ```

    注：这个配置文件命名，既可以是commitlint.config.js，也可以是`.commitlintrc.js`

3. 注册husky钩子

    ```bash
        npx husky install
        npx husky add .husky/commit-msg 'npx --no-install commitlint --edit "$1"'
    ```

4. 配置安装后自动启用husky钩子

    你可以使用下面这个命令

    ```bash
        npm set-script prepare "husky install"
    ```

    或者直接修改package.json

    ```json
        {
            // ...
            "scripts": {
                "prepare": "husky install"
            },
            // ...
        }
    ```

5. 低版本husky

如果你的node版本小于12，那么你按上述进行配置提交会报错。原因是新版本husky需要node版本最低是v12。

+ husky降级

    由于husky在v4版本之后，进行了破坏性改动，导致v4前后的版本不兼容，因此我们需要对husky的配置进行调整

    1. 卸载husky(对于.husky文件夹，可以删除，也可以保留，它已经没用了)

        ```bash
            npm uninstall husky && git config --unset core.hooksPath
        ```

    2. 安装旧版本husky

        `npm install husky@3.0.5 -D`

    3. 更改package.json文件  

        ```json
            {
                // ...
                "husky": {
                    "hooks": {
                    "commit-msg": "commitlint -e $HUSKY_GIT_PARAMS"
                    }
                },
                "devDependencies": {
                    "@commitlint/cli": "^13.1.0",
                    "@commitlint/config-conventional": "^13.1.0",
                    "husky": "^3.0.5"
                }
            }
        ```

这样，就可以正常的触发husky的钩子了，最后：[源码地址](https://github.com/fengluoX/lintDemo/tree/main)
