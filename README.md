# TVBox 接口仓库（tvbox2）

本仓库存放可直接导入 **TVBox / 影视仓** 的接口配置，所有文件自包含，从 GitHub 取数不依赖任何第三方站点。

## 文件说明

| 文件 | 说明 |
|------|------|
| `sun.json` | 已解密的标准 TVBox 配置，含 106 个采集站（`type=3` 的 csp 爬虫源）。 |
| `deps/spider.jar` | 全局 spider 爬虫包（md5=`cfab59e847b219aeb1806bff51a68a52`），由 `sun.json` 的 `spider` 字段引用，负责提供各站爬虫类。 |

## ⚠️ 国内访问说明（重点）

`raw.githubusercontent.com` 在国内直连不稳定 / 被墙，且有两个坑已替你规避：

1. **jsDelivr 会返回 `403` 拦截 `.jar` 文件** —— 所以 spider.jar 不能走 jsDelivr。
2. **ghproxy 类代理会改写响应体里的 URL**（把配置内 `spider` 还原成直连 raw，导致客户端拉不到 jar）—— 所以「订阅地址」不能走 ghproxy 嵌套。

本仓库采用的组合：**订阅地址走 gitmirror 镜像（国内直连、不经 ghproxy、不被改写），spider.jar 走 ghproxy.net（只有它能拉 jar）**。

### 主用（推荐）

订阅地址（填入 TVBox 配置地址）：

```
https://raw.gitmirror.com/hebijunge/tvbox2/main/sun.json
```

该配置内的 `spider.jar` 已自动指向代理地址，客户端加载后自动经 ghproxy 拉取，**无需手动下载**：

```
https://ghproxy.net/https://raw.githubusercontent.com/hebijunge/tvbox2/main/deps/spider.jar
```

### 备选方案（gitmirror 也不通时）

订阅改用 ghproxy 嵌套：

```
https://ghproxy.net/https://raw.githubusercontent.com/hebijunge/tvbox2/main/sun.json
```

> 注意：经 ghproxy 拉取订阅时，代理可能把响应里 `spider` 的地址还原成直连 raw；若客户端因此拉不到 jar，请把 `spider` 字段手动改回带 `https://ghproxy.net/` 前缀，或保持订阅用上面的 gitmirror 方案（推荐）。

jsDelivr 仅可作订阅地址备选（国内 CDN），但**不能用于 spider**（会 403 挡 jar）：

```
https://cdn.jsdelivr.net/gh/hebijunge/tvbox2@main/sun.json
```

## 使用方法

1. 打开 TVBox / 影视仓 → 设置 → 配置地址（订阅）。
2. 填入上面的「主用」订阅地址（`raw.gitmirror.com` 版）。
3. 保存，接口自动加载；`spider.jar` 由客户端按 `spider` 字段经代理自动拉取，**无需手动下载**。

## 备注

- `sun.json` 内 `wallpaper` / `danmaku` 指向 `127.0.0.1:9978`（本地代理运行时引用），需配套本地代理运行，不影响 106 个站点正常播放。
- `logo` 等图片来自外部图床，与 GitHub 直连无关，加载失败不影响使用。
- `spider.jar` 作者按日更新，配置内 md5 为锁死值；若某日失效，重新抓取并更新 md5 即可。
