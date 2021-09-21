
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
