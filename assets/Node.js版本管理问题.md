# Node.js 版本管理

### 升级 Node.js 版本

升级 Node.js 版本主要有以下几种方法，推荐使用第一种（nvm）：

1. **使用 Node Version Manager (nvm)** (推荐)
   nvm 允许你在系统中安装和管理多个 Node.js 版本，非常适合不同项目需要不同 Node.js 版本的场景。

   NVM 是 [node.js](https://nodejs.org/en/) 的版本管理器，设计为按用户安装，并按 shell 调用。 适用于任何符合 POSIX 的 shell（sh、dash、ksh、zsh、bash），特别是在以下平台上：unix、macOS 和 [windows WSL](https://github.com/nvm-sh/nvm#important-notes)。

   - **安装 nvm** (如果你还没有安装):
     你可以从 [nvm 的 GitHub 仓库](https://github.com/nvm-sh/nvm) 查看安装说明。

   - **通过 nvm 安装指定版本的 Node.js**:
     在终端中运行以下命令来安装 Node.js 22.12.0 或更高版本（例如 22.12.0）：

     ```bash
     nvm install 22.12.0
     ```

   - **使用新安装的版本**:

     ```bash
     nvm use 22.12.0
     ```

   - **(可选) 设置默认版本**:
     如果你希望新打开的终端都使用这个版本，可以运行：

     ```bash
     nvm alias default 22.12.0
     ```

     

2. **使用 Node.js 官方安装包**
   你可以直接从 [Node.js 官网](https://nodejs.org/) 下载并安装最新的稳定版（LTS）或当前版（Current）。安装器会自动覆盖旧版本。

3. **使用包管理器（如 Homebrew for macOS）**
   如果你使用的是 macOS 并且通过 Homebrew 安装的 Node.js，可以运行：

   ```bash
   brew update
   brew upgrade node
   ```

### 升级后的验证与可能的问题处理

升级到新版本的 Node.js 后：

1. **验证版本**：在终端再次运行 `node -v`，确认版本号已经符合 Vite 7.0 的要求（>=20.19 或 >=22.12）。

2. **重新安装依赖**：**强烈建议**删除项目的 `node_modules` 文件夹和 `package-lock.json`（或 `yarn.lock`）文件，然后重新运行 `npm install` 或 `yarn install`。这可以避免因 Node.js 版本变更可能导致的本地依赖编译问题。

   ```bash
   rm -rf node_modules package-lock.json
   npm install
   ```

   

3. **测试项目**：运行 `npm run dev` 和 `npm run build`，确保开发服务器能正常启动，生产构建也能成功完成。

4. **注意可能的 OpenSSL 相关问题**（如果从非常旧的版本升级）：极少数非常老的项目在升级到 Node.js 17+ 版本后，可能会因为 OpenSSL 3.0 的引入而遇到兼容性问题[9](https://devpress.csdn.net/vue/66cd66cb1338f221f9243c17.html)。如果遇到这类错误，可能需要通过环境变量 `NODE_OPTIONS="--openssl-legacy-provider"` 来临时解决，但**这只是临时方案，建议从根本上更新依赖或构建配置**。





**推荐使用 nvm 进行版本管理**，这让你未来切换版本更加灵活。升级后记得**重新安装项目依赖**并**进行全面测试**。



## NVM安装

### 对于 macOS 和 Linux 系统

#### 1. 使用官方安装脚本（推荐）

这是最快捷的安装方式。打开你的终端（Terminal），然后运行以下命令：

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

或者使用 `wget`：

```bash
wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

**请注意：** 命令中的 `v0.39.7` 是 nvm 的版本号，建议去 [nvm GitHub 官网](https://github.com/nvm-sh/nvm) 查看最新的版本号并替换。

#### 2. 配置环境变量

安装脚本会自动在你的 Shell 配置文件（如 `~/.bashrc`, `~/.zshrc`, `~/.profile`）末尾添加配置代码。为了让配置立即生效，你需要**重启终端**，或者运行对应的 source 命令：

- **如果你使用 Bash** (macOS 默认通常是 zsh，但旧版或 Linux 可能是 bash):

  ```bash
  source ~/.bashrc
  ```

- **如果你使用 Zsh** (macOS Catalina 及之后版本的默认 shell):

  ```bash
  source ~/.zshrc
  ```

  

#### 3. 验证安装

运行以下命令，如果出现版本号，说明安装成功。

```bash
nvm --version
```



### 对于 Windows 系统

在 Windows 上，原来的 nvm 不支持。但有两个非常优秀的替代项目，**nvm-windows** 是最流行的选择。

#### 1. 卸载现有 Node.js（非常重要！）

为了避免与现有安装冲突，**请先通过“添加或删除程序”彻底卸载当前版本的 Node.js**。nvm-windows 会自己管理一个独立位置的 Node.js 版本。

#### 2. 下载 nvm-windows 安装包

访问 **nvm-windows** 的官方发布页面：
https://github.com/coreybutler/nvm-windows/releases

下载最新版本的 `nvm-setup.exe` 安装程序（例如 `nvm-setup.exe`）。

#### 3. 运行安装程序

1. 运行下载的 `nvm-setup.exe`。
2. 接受许可协议。
3. 选择 nvm 的安装路径（例如 `C:\Users\YourName\AppData\Roaming\nvm`），使用默认路径即可。
4. 选择 Node.js 版本的符号链接（Symlink）路径（例如 `C:\Program Files\nodejs`）。**这个文件夹将是 nvm 用来切换当前激活版本的地方**。
5. 点击安装，完成即可。

#### 4. 验证安装

打开一个**新的**命令提示符（CMD）或 PowerShell 窗口，运行：

```bash
nvm --version
```



如果显示版本号，则安装成功。





### 安装并切换 Node.js 版本

nvm 安装成功后，你就可以安装并切换到 Vite 所需的 Node.js 版本了。

1. **安装所需的 Node.js 版本** (例如 `22.12.0`)

   ```bash
   nvm install 22.12.0
   ```

   

   你也可以安装最新的 LTS 版本：

   ```bash
   nvm install --lts
   ```

   

   或者安装最新发布版：

   ```bash
   nvm install latest
   ```

   

2. **使用该版本**

   ```bash
   nvm use 22.12.0
   ```

   

3. **（可选）设置默认版本**
   这样新打开的终端都会自动使用这个版本。

   ```bash
   nvm alias default 22.12.0
   ```

   

4. **验证 Node.js 版本是否已切换**

   ```bash
   node -v
   # 应该输出 v22.12.0 或你刚安装的其他版本号
   ```

   

### 常用 nvm 命令

| 命令                          | 描述                     |
| :---------------------------- | :----------------------- |
| `nvm ls`                      | 查看所有已安装的版本     |
| `nvm ls-remote`               | 查看所有可远程安装的版本 |
| `nvm install <version>`       | 安装指定版本的 Node.js   |
| `nvm use <version>`           | 切换到指定版本           |
| `nvm current`                 | 显示当前正在使用的版本   |
| `nvm alias default <version>` | 设置默认版本             |
| `nvm uninstall <version>`     | 卸载指定版本             |

现在，你的 Node.js 版本已经满足 Vite 7.0 的要求了。记得回到你的 React 项目目录下，**删除 `node_modules` 文件夹和 `package-lock.json` 文件后，重新运行 `npm install`**，以确保所有依赖都被正确安装。