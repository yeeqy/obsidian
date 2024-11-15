
Appium Desktop 已被弃用，现在将“检查器”（Inspector）功能移到了独立应用 **Appium Inspector** 中。因此，如果你需要在 Windows 上设置 Appium 测试环境，需要使用 **Appium Inspector** 与 **Appium Server** 配合使用。以下是如何安装和配置 Appium 2 的步骤：

### 1. 安装 Appium Server

Appium 2 不再提供桌面版，需要通过命令行安装。确保你已安装了 Node.js，因为 Appium 需要 Node.js 环境。

1. **安装 Node.js**：
   - 到 [Node.js 官网](https://nodejs.org/)下载并安装最新的 LTS 版本。

2. **安装 Appium 2**：
   - 打开命令行（如命令提示符或 PowerShell），输入以下命令来安装 Appium：
     ```bash
     npm install -g appium
     ```
   - 安装完成后，你可以输入以下命令来启动 Appium Server：
     ```bash
     appium --allow-cors
     ```
     `--allow-cors` 参数允许 Appium Inspector 通过浏览器远程访问本地的 Appium Server。

3. **确认安装成功**：
   - 在命令行中输入 `appium -v`，如果显示版本号，说明安装成功。
   - 运行 `appium` 命令后，Appium Server 将默认在端口 4723 启动，地址为 `http://127.0.0.1:4723`。

### 2. 下载并安装 Appium Inspector

Appium Inspector 可以直接从 Appium 的 [GitHub Releases 页面](https://github.com/appium/appium-inspector/releases)下载。

1. **选择适合你的系统的安装包**（例如 `.exe` 文件用于 Windows）。
2. **安装 Appium Inspector**：下载完成后，双击安装文件并按步骤完成安装。

### 3. 启动 Appium Inspector 并连接到本地的 Appium Server

1. 打开 **Appium Inspector**。
2. 在 “New Session” 部分，配置 Appium Server 连接信息：
   - **Remote Host**：输入 `127.0.0.1`
   - **Port**：输入 `4723`（或你启动 Appium Server 时指定的端口）
   - **Remote Path**：输入 `/wd/hub`
3. 配置 Desired Capabilities，例如：
   - **platformName**：`Android`
   - **deviceName**：Android 设备名称（可以通过 `adb devices` 获取）
   - **appPackage** 和 **appActivity**：指定要测试的 Android 应用的包名和启动 Activity。

4. 点击 “Start Session”，如果配置正确，Appium Inspector 将连接到你的 Android 设备，并允许你通过 Inspector 检查元素、录制测试步骤等。

### 4. 使用浏览器版 Appium Inspector（可选）

如果你不想安装 Appium Inspector 应用，也可以使用其浏览器版本：
- 打开 [Appium Inspector](https://inspector.appiumpro.com)。
- 启动 Appium Server 时使用 `--allow-cors` 标志。
- 在浏览器版 Appium Inspector 中输入服务器地址和 Desired Capabilities，然后点击 “Start Session”。

通过以上步骤，你就可以在 Windows 上配置 Appium Server 和 Appium Inspector，进行 Android 应用的自动化测试。