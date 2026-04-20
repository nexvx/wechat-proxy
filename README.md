# wechat-proxy

微信公众号 API 代理工具：支持 token 获取、图片上传、草稿创建和远程代理。

## 功能

- **Access Token 获取**：获取微信公众号 Access Token
- **图片上传**：上传图片到微信素材库
- **草稿创建**：创建微信公众号图文草稿
- **远程代理**：通过 HTTP 接口代理微信 API 调用
- **多appid支持**：支持配置多个微信公众号应用

## 安装

```bash
npm install
```

需要 Node.js 18+。

## 配置

需要微信公众号凭证。

**环境变量：**

```bash
export WECHAT_APP_ID=your_appid
export WECHAT_SECRET=your_secret
```

**或配置文件**（按优先级从高到低）：

```
./wechat-proxy.yaml
~/.wechat-proxy.yaml
~/.config/wechat-proxy/config.yaml
```

**单appid配置：**

```yaml
wechat:
  appid: your_appid
  secret: your_secret
```

**多appid配置：**

```yaml
wechat:
  apps:
    default:
      appid: your_default_appid
      secret: your_default_secret
    app2:
      appid: your_second_appid
      secret: your_second_secret
```

## 使用

接口请求支持通过请求体参数 `appid` 指定使用的 appid 配置，默认值为 `default`。

示例：获取 Access Token
```bash
curl -X POST http://localhost:8080/api/access-token \
  -H 'Content-Type: application/json' \
  -H 'X-Proxy-Secret: your-proxy-secret' \
  -d '{"appid":"app2"}'
```

示例：创建草稿
```bash
curl -X POST http://localhost:8080/api/create-draft \
  -H 'Content-Type: application/json' \
  -H 'X-Proxy-Secret: your-proxy-secret' \
  -d '{"appid":"app2","label":"图文草稿","articles":[{...}]}'
```

示例：上传图片素材
```bash
curl -X POST http://localhost:8080/api/upload-material \
  -H 'X-Proxy-Secret: your-proxy-secret' \
  -F 'appid=app2' \
  -F 'media=@/path/to/image.jpg'
```

详见 [SKILL.md](./SKILL.md)。

## 构建 Server Mode 可执行文件

如果你只想部署远程代理服务，可以单独将 `proxy-server.mjs` 打包成单个可执行文件。

本地构建要求：
- Node.js 25.5.0+
- 执行 `npm install`

```bash
npm run build:proxy-server:sea
```

构建输出：
- `dist/wechat-proxy`（Linux/macOS）
- `dist/wechat-proxy.exe`（Windows）

运行方式：

```bash
./dist/wechat-proxy --port 8080 --secret your-proxy-secret
```

仓库内置了 GitHub Actions 工作流 [build-proxy-server-sea.yml](./.github/workflows/build-proxy-server-sea.yml)，可在 GitHub 上生成以下平台的 artifact：
- Linux x64
- macOS arm64
- Windows x64

## 可用主题

| 主题 | 说明 |
|------|------|
| `autumn-warm` | 秋日暖光，橙色调（默认） |
| `spring-fresh` | 春日清新，绿色调 |
| `ocean-calm` | 海洋静谧，蓝色调 |

## 项目结构

```
scripts/
  convert.mjs           # Markdown → 微信 HTML
  upload-image.mjs      # 上传本地图片
  download-upload.mjs   # 下载在线图片并上传
  replace-images.mjs    # 替换 HTML 占位符
  publish.mjs           # 一键：Markdown → 替换图片 → 最终 HTML
  create-draft.mjs      # 创建图文草稿
  create-image-post.mjs # 创建小绿书

lib/                    # 可复用模块（脚本内部使用）
themes/                 # 主题 YAML 文件
writers/                # 写作风格 YAML 文件
```
