# TVBox 接口仓库（tvbox2）

本仓库存放可直接导入 **TVBox / 影视仓** 的接口配置。所有文件自包含，从 GitHub 取数不依赖任何第三方站点。

## 文件说明

| 文件 | 说明 |
|------|------|
| `sun.json` | 已解密的标准 TVBox 配置，含 106 个采集站（`type=3` 的 csp 爬虫源）。 |
| `deps/spider.jar` | 全局 spider 爬虫包（md5=`cfab59e847b219aeb1806bff51a68a52`），由 `sun.json` 的 `spider` 字段引用，负责提供各站爬虫类。 |

## ⚠️ 必须走代理（重点）

`raw.githubusercontent.com` 在国内直连不稳定 / 被墙，**订阅地址和 spider jar 都必须经过 GitHub 代理**才能拉取。本仓库默认已把代理前缀内嵌好，开箱即用；如遇某个代理失效，按下方「代理前缀替换」改一下即可。

### 主用：ghproxy.net（订阅 + jar 都走它）

```
订阅地址：https://ghproxy.net/https://raw.githubusercontent.com/hebijunge/tvbox2/main/sun.json
```

`spider.jar` 已自动指向：

```
https://ghproxy.net/https://raw.githubusercontent.com/hebijunge/tvbox2/main/deps/spider.jar
```

> 注意：`spider.jar` **只能走 ghproxy 系代理**。实测 jsDelivr 会返回 `403` 拦截 `.jar` 文件，请勿把 jar 改到 jsDelivr。

### 代理前缀替换（某个代理不通时）

把地址里 `https://ghproxy.net/https://` 这一整段替换成以下任一：

| 代理 | 替换串 |
|------|--------|
| ghproxy.net（默认） | `https://ghproxy.net/https://` |
| ghproxy.com | `https://ghproxy.com/https://` |
| mirror.ghproxy | `https://mirror.ghproxy.com/https://` |

例：若 `ghproxy.net` 不通，订阅地址改为

```
https://ghproxy.com/https://raw.githubusercontent.com/hebijunge/tvbox2/main/sun.json
```

### 订阅地址的备选：jsDelivr（仅 sun.json，不含 jar）

若只是想拉 `sun.json` 且 ghproxy 全不通，可用 jsDelivr CDN（国内直连较快）：

```
https://cdn.jsdelivr.net/gh/hebijunge/tvbox2@main/sun.json
```

但**此地址仅能拉配置本身**；配置内的 `spider.jar` 仍必须走上面任一 ghproxy 代理，否则客户端加载后拉不到爬虫包。

## 使用方法

1. 打开 TVBox / 影视仓 → 设置 → 配置地址（订阅）。
2. 填入上面的「订阅地址」（ghproxy.net 版）。
3. 保存，接口自动加载；`spider.jar` 由客户端按 `spider` 字段经代理自动拉取，**无需手动下载**。

## 备注

- `sun.json` 内 `wallpaper` / `danmaku` 指向 `127.0.0.1:9978`（本地代理运行时引用），需配套本地代理运行，不影响 106 个站点正常播放。
- `logo` 等图片来自外部图床，与 GitHub 直连无关，加载失败不影响使用。
- `spider.jar` 作者按日更新，配置内 md5 为锁死值；若某日失效，重新抓取并更新 md5 即可。
