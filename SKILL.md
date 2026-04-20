---
name: wechat-studio
description: 微信公众号 API 代理工具，支持 token 获取、图片上传、草稿创建和远程代理。Use this skill when the user asks about WeChat Official Account API proxy, including token, upload, draft creation, and remote proxy.
---

# WeChat Studio

微信公众号 API 代理工具：支持 token 获取、图片上传、草稿创建和远程代理。

## 路径解析（重要）

本 skill 的脚本位于 SKILL.md 所在目录下。执行任何脚本之前，**必须**先确定本文件的实际路径，并以此推导脚本目录。

**规则：** 以下文档中所有 `$SKILL_DIR` 占位符应替换为本 SKILL.md 文件所在的目录路径。

例如：如果本文件位于 `/Users/xxx/Desktop/AI/skills/wechat-studio/SKILL.md`，则 `$SKILL_DIR` = `/Users/xxx/Desktop/AI/skills/wechat-studio`。

```bash
SKILL_DIR="<本 SKILL.md 文件所在目录的绝对路径>"
```

---

## 功能：微信 API 代理

### 代理服务器

启动远程代理服务器，暴露 API 接口供客户端调用。

```bash
# 启动服务器
node "$SKILL_DIR/scripts/proxy-server.mjs" --port 8080 --secret your-proxy-secret

# 或使用环境变量
PROXY_PORT=8080 PROXY_SECRET=your-proxy-secret node "$SKILL_DIR/scripts/proxy-server.mjs"
```

**接口：**
- `POST /api/access-token` - 获取 Access Token
- `POST /api/upload-material` - 上传图片素材
- `POST /api/create-draft` - 创建图文草稿

### 客户端脚本

#### 获取 Access Token

```bash
node "$SKILL_DIR/scripts/download-upload.mjs" token
```

#### 上传图片

```bash
# 单张
node "$SKILL_DIR/scripts/upload-image.mjs" /path/to/image.jpg

# 批量
node "$SKILL_DIR/scripts/upload-image.mjs" --json images.json
```

#### 创建草稿

```bash
node "$SKILL_DIR/scripts/create-draft.mjs" --title "标题" --content "<p>内容</p>" --thumb-media-id "media_id"
```

---

## 配置

需要配置微信公众号的 appid 和 secret，见 `lib/config.mjs`。
