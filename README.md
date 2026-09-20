# tvbox2 — 自包含 TVBox 接口仓库

本仓库收录**可直接从 GitHub 访问、依赖自包含**的 TVBox / 影视仓接口配置，手机端无需直连 github。

## 文件结构
```
sun.json              接口配置（106 个 csp 爬虫源）
deps/spider.jar       唯一外部依赖：爬虫 jar（md5=cfab59e847b219aeb1806bff51a68a52）
README.md             本文件
```

## 订阅地址（填进 TVBox 配置地址即可）
手机访问不了 raw.githubusercontent.com，请用镜像地址：

- **主用（gitmirror 镜像，国内直连、不改写内容）**
  ```
  https://raw.gitmirror.com/hebijunge/tvbox2/main/sun.json
  ```
- **备选（gh.927223.xyz 透传镜像，同样不改写 spider）**
  ```
  https://gh.927223.xyz/https://raw.githubusercontent.com/hebijunge/tvbox2/main/sun.json
  ```

## spider.jar 加载（已内嵌在 sun.json，无需手动配置）
`sun.json` 的 `spider` 字段已指向最快镜像，客户端首次加载会自动下载 jar：
```
https://gh.927223.xyz/https://raw.githubusercontent.com/hebijunge/tvbox2/main/deps/spider.jar;md5;cfab59e847b219aeb1806bff51a68a52
```
> 实测下载速率对比（1.87MB jar）：`gh.927223.xyz` ≈ 1.7 MB/s（1 秒拉完）、`ghf.xn--eqrr82bzpe.top` ≈ 1.67 MB/s，二者均为**透传型、不改写响应 URL**；
> 而 `ghproxy.net` 仅 ≈ 0.36 MB/s（慢 5 倍）且会**改写 spider 为直连 raw**，故不推荐。

## 注意事项
- **jsDelivr 不能用于 jar**：`cdn.jsdelivr.net` 对 `.jar` 返回 403，仅适合拉 JSON 订阅。
- **本地代理**：配置里的 `wallpaper` / `danmaku` 指向 `127.0.0.1:9978`（本地代理运行时引用），需配套代理在运行，否则仅壁纸/弹幕不显示，不影响 106 个站点播放。
- **jar 时效**：spider.jar 由上游按日更新，配置里 md5 锁死。若某天加载报 md5 不符，重新抓取替换 `deps/spider.jar` 并更新 md5 即可。
