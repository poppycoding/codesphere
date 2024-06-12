## 教程：如何在 Mac 安装 Oh My Zsh

Oh My Zsh 是一个流行的命令行工具，用于管理 Zsh shell 的配置和插件。以下是在 macOS 上安装 Oh My Zsh 的简短教程：

### 步骤 1：安装 Zsh

如果你尚未安装 Zsh，可以通过 Homebrew 快速安装：

```sh
brew install zsh
```

### 步骤 2：安装 Oh My Zsh

1. 使用 curl 安装 Oh My Zsh。在终端中运行以下命令：

    ```sh
    sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
    ```

    或者，如果你更喜欢使用 wget，可以运行：

    ```sh
    sh -c "$(wget -O- https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
    ```

2. 运行上述命令后，会提示你是否继续安装 Oh My Zsh。输入 `Y` 然后按下回车键。

3. 安装完成后，终端会自动切换到 Zsh shell。

### 步骤 3：iterms 字体乱码, 需要安装 powerline 字体然后进行切换

#先使用git命令克隆
git clone https://github.com/powerline/fonts.git --depth=1

# 进入克隆到本地的fonts目录进行安装
cd fonts
./install.sh

# 删除克隆到本地的目录
cd .. 
rm -rf fonts


## 教程：如何在 Mac 终端中开启和关闭代理

在某些情况下，我们需要通过代理服务器访问互联网。这篇教程将教你如何在 Mac 终端中开启和关闭代理。我们将创建两个便捷的函数来管理代理设置，并将它们添加到你的 `~/.bash_profile` 文件中。

#### 第一步：编辑 `~/.bash_profile`

我们将两个新函数 `proxy_on` 和 `proxy_off` 添加到 `~/.bash_profile` 文件中。这两个函数分别用于开启和关闭代理设置。

打开终端，运行以下命令：

```sh
cat >> ~/.bash_profile << EOF
function proxy_on() {
    export http_proxy=http://127.0.0.1:7890
    export https_proxy=\$http_proxy
    echo -e "终端代理已开启。"
}

function proxy_off(){
    unset http_proxy https_proxy
    echo -e "终端代理已关闭。"
}
EOF

source ~/.bash_profile
```

#### 解释脚本内容

1. **添加函数到 `~/.bash_profile`**：
    ```sh
    cat >> ~/.bash_profile << EOF
    function proxy_on() {
        export http_proxy=http://127.0.0.1:7890
        export https_proxy=\$http_proxy
        echo -e "终端代理已开启。"
    }

    function proxy_off(){
        unset http_proxy https_proxy
        echo -e "终端代理已关闭。"
    }
    EOF
    ```

    这段代码将 `proxy_on` 和 `proxy_off` 函数添加到 `~/.bash_profile` 文件中：
    - **`proxy_on`** 函数：设置代理，`http_proxy` 和 `https_proxy` 环境变量都指向 `http://127.0.0.1:7890`。
    - **`proxy_off`** 函数：取消代理设置，清除 `http_proxy` 和 `https_proxy` 环境变量。

2. **重新加载 `~/.bash_profile`**：
    ```sh
    source ~/.bash_profile
    ```
    这行命令将使新的配置立即生效，无需重启终端。

#### 第二步：使用代理函数

现在你可以使用 `proxy_on` 和 `proxy_off` 函数来管理终端代理。

- **开启代理**：在终端中运行
    ```sh
    proxy_on
    ```
    这将设置代理，并显示 "终端代理已开启。"

- **关闭代理**：在终端中运行
    ```sh
    proxy_off
    ```
    这将清除代理设置，并显示 "终端代理已关闭。"

### 教程：如何在 Mac 中搭建 Node.js 环境

Node.js 是一个基于 Chrome V8 引擎的 JavaScript 运行环境，广泛用于构建服务器端应用程序。本文将指导你如何在 Mac 上安装和配置 Node.js 环境。

#### 第一步：安装 Homebrew

Homebrew 是 macOS 上的包管理器，可以方便地安装和管理各种软件包。

1. 打开终端。
2. 运行以下命令来安装 Homebrew：
    ```sh
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    ```

3. 安装完成后，按照提示配置环境变量，通常是将以下命令添加到 `~/.bash_profile` 或 `~/.zshrc` 文件中：
    ```sh
    echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
    eval "$(/opt/homebrew/bin/brew shellenv)"
    ```

#### 第二步：安装 Node.js

1. 安装完成 Homebrew 后，在终端中运行以下命令来安装 Node.js：
    ```sh
    brew install node
    ```

2. 安装完成后，你可以使用以下命令来验证 Node.js 和 npm（Node.js 包管理器）的安装是否成功：
    ```sh
    node -v
    npm -v
    ```
    如果成功，命令会返回 Node.js 和 npm 的版本号。

#### 第三步：配置 Node.js 环境

Node.js 和 npm 已经成功安装并配置，现在可以开始使用它们。

1. **创建一个新项目**：
    ```sh
    mkdir my-node-project
    cd my-node-project
    ```

2. **初始化 npm 项目**：
    ```sh
    npm init -y
    ```
    这会生成一个 `package.json` 文件，包含项目的基本信息。

3. **安装项目依赖**（例如 Express 框架）：
    ```sh
    npm install express
    ```

#### 第四步：编写和运行简单的 Node.js 应用

1. 在项目目录中创建一个名为 `app.js` 的文件，并添加以下代码：
    ```javascript
    const express = require('express');
    const app = express();
    const port = 3000;

    app.get('/', (req, res) => {
        res.send('Hello World!');
    });

    app.listen(port, () => {
        console.log(`Example app listening at http://localhost:${port}`);
    });
    ```

2. 在终端中运行以下命令来启动应用：
    ```sh
    node app.js
    ```

3. 打开浏览器，访问 `http://localhost:3000`，你应该会看到 "Hello World!"。

