# uiautodev
[![codecov](https://codecov.io/gh/codeskyblue/appinspector/graph/badge.svg?token=aLTg4VOyQH)](https://codecov.io/gh/codeskyblue/appinspector)
[![PyPI version](https://badge.fury.io/py/uiautodev.svg)](https://badge.fury.io/py/uiautodev)

官网: https://uiauto.dev

UI Inspector for Android, iOS and Harmony.  
支持元素查看、属性面板、定位语句生成与验证，并可在离线环境运行。

## 新功能说明

- 新增 `UI Inspector` 页面: `http://127.0.0.1:20242/inspector`
  - React + Tailwind 三栏布局，支持左右分栏拖拽调整占比
  - 左侧 Canvas 设备镜像自适应缩放，支持 hover/选中高亮
  - 中间层级树支持折叠、展开、选中联动
  - 右侧属性面板和定位语句实时联动

- Android 真实数据接入（替换 mock 数据）
  - 设备列表: `/api/android/list`
  - JSON 层级: `/api/android/{serial}/hierarchy?format=json`
  - XML 层级: `/api/android/{serial}/hierarchy?format=xml`
  - 截图流: `/api/android/{serial}/screenshot/0`

- 新增 `Element Search Validator`（查找与验证模态框）
  - 支持策略: `XPath` / `ID(resource-id)` / `Accessibility ID(content-desc)` / `Class Name` / `u2_selector(JSON)`
  - 搜索结果支持点击回填，并联动左侧 Canvas 高亮和右侧属性面板

- `u2_selector` 生成器增强
  - 输出统一数组格式: `["u2_selector", {...}]`
  - 支持唯一性校验与 fallback 组合策略

- 离线缓存目录内置到项目静态资源
  - 缓存路径已迁移为: `uiautodev/static/cache/http`
  - `--offline` 模式下缓存未命中会直接返回 404，不再访问公网

## 安装

```bash
# PyPI 安装
pip install uiautodev

# Harmony 支持
pip install "uiautodev[harmony]"
```

```bash
# 本地开发安装（推荐用于修改源码后立即生效）
pip install -e .
```

## 快速启动

```bash
# 默认启动本地服务并自动打开浏览器
uiautodev server
```

常用参数:

- `--host 0.0.0.0`: 局域网访问
- `--port 20242`: 指定端口
- `--no-browser`: 不自动打开浏览器
- `--offline`: 仅使用本地缓存

## 部署手册

### 1) 在线部署（常规）

```bash
pip install uiautodev
uiautodev server --host 0.0.0.0 --port 20242
```

访问:

- 首页: `http://<server-ip>:20242/`
- Inspector: `http://<server-ip>:20242/inspector`

### 2) 离线部署（推荐先预热缓存）

1. 联网预热一次缓存:

```bash
uiautodev server
```

在浏览器中打开并浏览主要页面（Android/iOS/Harmony/Inspector），确保静态资源全部下载。

2. 确认缓存目录:

- `uiautodev/static/cache/http/` 下应已有 `.body/.headers` 文件

3. 断网或隔离网络后启动:

```bash
uiautodev server --offline --host 0.0.0.0 --port 20242 --no-browser
```

说明:

- `--offline` 会自动开启环境变量 `UIAUTODEV_OFFLINE=true`
- 离线模式下若缓存未命中，接口会返回 `Offline mode: cache miss`

### 3) 源码部署（适合二次开发）

```bash
git clone <your-repo-url>
cd uiautodev_u2
pip install -e .
uiautodev server --host 0.0.0.0 --port 20242
```

## 环境变量

```sh
# 默认 Android 驱动为 uiautomator2，设置后切换到 adb driver
export UIAUTODEV_USE_ADB_DRIVER=1
```

```sh
# 可选：手动强制离线（通常使用 --offline 即可）
export UIAUTODEV_OFFLINE=true
```

## 常见问题

- `/inspector` 打开 404
  - 请确认当前运行的是本仓库代码，而不是旧版 site-packages。
  - 建议执行: `pip install -e .` 后重启服务。

- 离线页面资源不完整
  - 先在联网模式打开所有主要页面完成缓存预热，再使用 `--offline` 启动。

## 开发

详见 [DEVELOP.md](DEVELOP.md)

## Links

- https://app.tangoapp.dev/ 基于 webadb 的手机远程控制项目
- https://docs.tangoapp.dev/scrcpy/video/web-codecs/ H264 解码器

## License

[MIT](LICENSE)
