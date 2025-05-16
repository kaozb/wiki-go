

# LeoMoon Wiki-Go

![docker builds](https://github.com/leomoon-studios/wiki-go/actions/workflows/release-docker.yml/badge.svg)
![binary builds](https://github.com/leomoon-studios/wiki-go/actions/workflows/release_binaries.yml/badge.svg)
[![version](https://img.shields.io/github/v/release/leomoon-studios/wiki-go?label=Version)](https://github.com/leomoon-studios/wiki-go/releases)
[![demo](https://img.shields.io/badge/demo-available-brightgreen)](https://wikigo.leomoon.com)

![Desktop](screenshots/preview.png)

LeoMoon Wiki-Go 是一个现代化的、功能丰富的、**无数据库的扁平文件 Wiki** 平台，使用 Go 语言构建。它提供了一个简洁、直观的界面，用于创建和管理知识库、文档和协作内容，而无需任何外部数据库。

无需数据库。无需臃肿。零维护。只需 Markdown。

## 非 SSL 设置的重要配置说明

如果你在没有 SSL/HTTPS 的情况下运行 Wiki-Go 并且遇到登录问题，你需要在你的 `config.yaml` 文件中设置 `allow_insecure_cookies: true` 并重启 Wiki-Go。 这是因为：

1. 默认情况下，Wiki-Go 为了安全起见，会在 cookie 上设置 "Secure" 标志
2. 浏览器会拒绝在非 HTTPS 连接上设置 "Secure" 标志的 cookie
3. 这会阻止登录在仅 HTTP 设置中正常工作

> **安全提示**：仅在开发环境或受信任的内部网络中使用此设置。 对于面向公众的 Wiki，请始终使用 HTTPS。

## 特性

### 功能概览

- ✍️ 完整的 Markdown 编辑，支持 emoji 表情、Mermaid 图表和 LaTeX 数学公式
- 🔍 智能全文搜索，具有高亮显示和高级过滤器
- 📁 层次化的页面结构，具有版本历史记录
- 👥 用户管理、访问控制和私有 Wiki 模式
- 💬 评论功能，支持审核和 Markdown 格式
- ⚡ 通过 Docker 或预构建的二进制文件实现快速安装
- 🧩 自定义 Logo、横幅、短代码等

> 适用于内部文档、个人知识库或团队 Wiki。

### 内容管理
- **Markdown 支持**: 使用 Markdown 语法编写内容，实现丰富的格式化
- **Emoji 短代码**: 在 Markdown 内容中使用 emoji 短代码，例如 `:smile:`
- **文件附件**: 上传和管理图片和文档 (支持 jpg, jpeg, png, gif, svg, txt, log, csv, zip, pdf, docx, xlsx, pptx, mp4)
- **分层组织**: 在嵌套目录中组织内容
- **版本历史**: 通过完整的修订历史记录跟踪更改，并恢复以前的版本
- **文档管理**: 使用用户友好的界面创建、编辑和删除文档
- **文档排序和命名**: 通过 slug 名称控制侧边栏中文档的顺序:
  - 文档按其目录 slug 名称按字母顺序排序
  - 文档标题（显示在侧边栏和标题中）取自 document.md 中的第一个 H1 标题
  - 要手动对文档进行排序，请在 slug 名称前加上数字（例如，`1-overview`，`2-installation`，`3-usage`）
  - slug 名称可以与显示的标题不同，从而允许组织化的结构，同时保持可读的标题
  - 示例：名为 `1-getting-started` 的目录，其中 document.md 包含 `# Getting Started Guide` 将在侧边栏中显示为“Getting Started Guide”，但会首先排序

### 协作 & 反馈
- **评论系统**: 启用文档讨论的全功能评论系统
- **评论中的 Markdown**: 使用与文档中相同的 Markdown 语法格式化评论
- **评论审核**: 管理员可以删除不适当的评论
- **禁用评论**: 可以通过 Wiki 设置在系统范围内禁用评论系统

### 搜索 & 导航
- **全文搜索**: 强大的搜索功能，支持:
  - 精确的短语匹配 (使用引号)
  - 术语的包含/排除
  - 高亮显示的搜索结果
- **面包屑导航**: 清晰的路径可视化，方便导航
- **侧边栏导航**: 快速访问文档层次结构

### 用户体验
- **响应式设计**: 适用于桌面和移动设备
- **暗黑/浅色主题**: 在暗黑和浅色模式之间切换
- **代码语法高亮**: 支持多种编程语言
- **数学渲染**: 通过 MathJax 支持 LaTeX 数学公式
- **图表**: 集成 Mermaid 图表，用于创建流程图、序列图等

### 管理
- **用户管理**: 创建和管理具有不同权限级别的用户
- **管理面板**: 通过 Web 界面配置 Wiki 设置
- **统计**: 跟踪文档指标和站点使用情况

### 高级功能
- **自定义短代码**: 使用特殊短代码（例如 `:::stats recenter=5:::`）扩展 Markdown，以获得更多功能
- **媒体嵌入**: 在文档中嵌入图像、视频和其他媒体
- **打印友好**: 优化的文档打印支持
- **API 访问**: RESTful API，用于以编程方式访问 Wiki 内容

## 演示站点

你可以使用下面的实时演示来试用 Wiki-Go。 演示站点每**小时**重置一次，所有上传或编辑的内容将被自动清除。

- 🔗 URL: https://wikigo.leomoon.com
- 👤 用户: `admin`
- 🔒 密码: `demo123`

## 快速开始

### Docker (快速测试)

```bash
# 拉取最新的镜像
docker pull leomoonstudios/wiki-go

# 使用默认配置运行
docker run -d \
  --name wiki-go \
  -p 8080:8080 \
  -v "$(pwd)/data:/wiki/data" \
  leomoonstudios/wiki-go
```

### Docker Compose

#### 选项 1 – 纯 HTTP (端口 8080)

使用提供的 `docker-compose-http.yml`:

```bash
docker-compose -f docker-compose-http.yml up -d
```

这将在 http://localhost:8080 上启动 Wiki-Go。 当你在反向代理（Nginx/Traefik/Caddy）上终止 TLS 时，这是理想的选择。 如果代理 --> 容器的跳转是纯 HTTP，请记住在 `data/config.yaml` 中设置 `allow_insecure_cookies: true`。

<details>
<summary>Nginx 反向代理配置 (点击展开)</summary>

```nginx
server {
    listen 80;
    server_name wiki.example.com;

    # 将所有 HTTP 重定向到 HTTPS（假设你在 443 上使用 Let's Encrypt）
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl http2;
    server_name wiki.example.com;

    ssl_certificate     /etc/letsencrypt/live/wiki.example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/wiki.example.com/privkey.pem;

    # --- 代理到在 HTTP 上运行的 Wiki-Go 容器（端口 8080）---
    location / {
        proxy_pass http://wiki-go:8080;

        # 推荐的标头
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;
    }
}
```

Nginx 服务的 Compose 示例：

```yaml
  nginx:
    image: nginx:alpine
    container_name: wiki-nginx
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/conf.d/default.conf:ro
      - /etc/letsencrypt:/etc/letsencrypt:ro
    depends_on:
      - wiki-go
```

</details>

#### 选项 2 – 原生 HTTPS (端口 443)

```bash
# 将证书 + 密钥放在 ./ssl/ 中
mkdir -p ssl
docker-compose -f docker-compose-ssl.yml up -d
```

`docker-compose-ssl.yml` 将主机端口 443 → 容器端口 443 映射，并挂载你的证书/密钥。 在应用程序配置中启用 TLS（见下文）。

---

## 原生 TLS 配置

在 `data/config.yaml` 中设置：

```yaml
server:
  host: 0.0.0.0
  port: 443            # 容器监听 443
  allow_insecure_cookies: false
  ssl: true            # 启用内置 HTTPS
  ssl_cert: /path/to/certificate.crt
  ssl_key:  /path/to/private.key
```

如果 `ssl: false` (默认)，该应用会在 `port` 上提供纯 HTTP（默认是 8080），你可以改为在反向代理后面运行它。

GitHub 发布的 Docker 镜像公开了 **8080 和 443 两个端口**，因此你可以在运行时选择任何一种场景。

---

### 二进制文件

从 [GitHub Releases](https://github.com/leomoon-studios/wiki-go/releases) 页面下载适用于你平台的最新版本。
```bash
# 运行应用程序
./wiki-go  # or wiki-go.exe on Windows
```

### 从源代码构建

要求：
- Go 1.21 或更高版本
- Git

```bash
# 克隆仓库
git clone https://github.com/leomoon-studios/wiki-go.git
cd wiki-go

# 构建二进制文件
go build -o wiki-go

# 运行应用程序
./wiki-go  # or wiki-go.exe on Windows
```

## 配置

### 基本设置

配置存储在 `data/config.yaml` 中，首次运行时会自动使用默认值创建该文件。 你可以修改此文件以自定义你的 Wiki：

```yaml
server:
    host: 0.0.0.0
    port: 8080
    # When set to true, allows cookies to be sent over non-HTTPS connections.
    # WARNING: Only enable this in trusted environments like a homelab
    # where HTTPS is not available. This reduces security by allowing
    # cookies to be transmitted in plain text.
    allow_insecure_cookies: true
    # Enable native TLS. When true, application will run over HTTPS using the
    # supplied certificate and key paths.
    ssl: false
    ssl_cert:
    ssl_key:
wiki:
    root_dir: data
    documents_dir: documents
    title: "📚 Wiki-Go"
    owner: wiki.example.com
    notice: Copyright 2025 © All rights reserved.
    timezone: America/Vancouver
    private: false
    disable_comments: false
    max_versions: 10
    # Maximum file upload size in MB
    max_upload_size: 10
    # Default language for the wiki interface (en, es, etc.)
    language: en
security:
    login_ban:
        # Enable protection against brute force login attacks
        enabled: true
        # Number of failed attempts before triggering a ban
        max_failures: 3
        # Time window in seconds for counting failures
        window_seconds: 30
        # Duration in seconds for the first ban
        initial_ban_seconds: 60
        # Maximum ban duration in seconds (24 hours)
        max_ban_seconds: 86400
users:
    - username: admin
      password: <bcrypt-hashed-password>
      is_admin: true
```

### 自定义

#### 自定义 Favicon

LeoMoon Wiki-Go 带有默认的 favicon，但你可以轻松地用你自己的替换它们：

1. 要使用自定义的 favicon，请将你的文件放在 `data/static/` 目录中，并使用以下名称：
   - `favicon.ico` - 标准 favicon 格式（供较旧的浏览器使用）
   - `favicon.png` - PNG 格式的 favicon
   - `favicon.svg` - SVG 格式的 favicon（建议用于在所有尺寸下获得最佳质量）

2. 应用程序将自动检测并使用你的自定义 favicon 文件，而无需重启。

建议使用 SVG 格式作为 favicon，因为它可以在不同的尺寸下很好地缩放，同时保持清晰的质量。

#### 自定义 Logo (可选)

你可以添加一个自定义 Logo 以显示在侧边栏的 Wiki 标题上方：

1. 使用受支持的格式之一创建一个 Logo 文件：
   - `logo.svg` - SVG 格式（建议用于获得最佳质量）
   - `logo.png` - PNG 格式（替代选项）

2. 将 Logo 文件放在 `data/static/` 目录中。

3. 该 Logo 将自动显示在侧边栏的 Wiki 标题上方。

**注意：**
- Logo 以 120×120 像素显示，但会保持其纵横比
- 建议使用 SVG 格式，以便在所有屏幕尺寸下获得最佳外观
- 无需配置更改或应用程序重启
- 如果不存在 Logo 文件，则只会显示 Wiki 标题
- 如果 logo.svg 和 logo.png 都存在，则将使用 logo.svg

#### 全局横幅 (可选)

你可以添加一个横幅图片，该图片将显示在所有文档的顶部：

1. 使用受支持的格式之一创建一个横幅图片：
   - `banner.png` - PNG 格式（建议用于获得最佳质量）
   - `banner.jpg` - JPG 格式（替代选项）

2. 将横幅文件放在 `data/static/` 目录中。

3. 该横幅将自动显示在所有文档内容的顶部。

**注意：**
- 横幅以响应式宽度和最大高度 250px 显示
- 横幅在适应不同的屏幕尺寸时会保持其纵横比
- 无需配置更改或应用程序重启
- 要删除横幅，只需从 `data/static/` 目录中删除该文件
- 如果 banner.png 和 banner.jpg 都存在，则将使用 banner.png

### 用户管理

LeoMoon Wiki-Go 包含一个用户管理系统，具有不同的权限级别：

- **管理员用户**: 可以创建、编辑和删除内容、管理用户和更改设置
- **普通用户**: 可以查看内容（在私有模式下）

默认的管理员凭据是：
- 用户名: `admin`
- 密码: `admin`

建议在首次登录后立即更改这些凭据。

## 安全

- **身份验证**: 使用安全密码哈希的用户身份验证
- **登录速率限制**: 在多次尝试失败后，通过临时 IP 封禁来防止暴力攻击
- **私有模式**: 可选的需要登录的私有 Wiki 模式
- **管理控制**: 用于内容管理的单独管理权限

## 使用

### 创建内容

1. 使用管理员凭据登录
2. 使用“新建”按钮创建一个新文档
3. 使用 Markdown 语法编写内容
4. 保存你的文档

### 组织内容

LeoMoon Wiki-Go 允许你以分层结构组织内容：

1. 创建目录以对相关文档进行分组
2. 在编辑模式下使用移动/重命名功能来重组内容
3. 使用侧边栏或面包屑导航浏览你的内容

### 附加文件

你可以将文件附加到任何文档：

1. 导航到文档并进入编辑模式
2. 单击“附件”
3. 使用上传按钮上传文件
4. 使用“文件”选项卡在文档中插入指向文件的链接

### 使用评论

评论系统允许用户提供反馈并参与讨论：

1. 导航到任何文档
2. 滚动到底部的评论部分
3. 经过身份验证的用户可以使用 Markdown 语法添加评论
4. 管理员可以删除任何评论
5. 可以通过管理设置面板在系统范围内禁用评论

## 技术细节

### 构建工具
- **后端**: Go (Golang)
- **前端**: HTML, CSS, JavaScript
- **编辑器**: [CodeMirror5](https://github.com/codemirror/codemirror5) 用于 Markdown 编辑
- **语法高亮**: [Prism.js](https://github.com/PrismJS/prism)
- **图表**: [Mermaid.js](https://github.com/mermaid-js/mermaid)
- **数学渲染**: [MathJax](https://github.com/mathjax/MathJax)

### 架构
- **简单的配置**: 简单的基于 YAML 的配置
- **基于文件的存储**: 文档存储为 Markdown 文件
- **轻量级 & 快速**: 为性能而构建
- **无外部数据库**: 独立的基于文件的存储

### 文件夹结构

Wiki-Go 使用一个简单的扁平文件结构来存储所有内容：

```
data/
├── config.yaml                   # Main configuration file for Wiki-Go
├── documents/                    # Regular wiki documents
│   └── path/
│       └── to/
│           └── doc-name/         # Document directory named "doc-name"
│               └── document.md   # The actual markdown content for "doc-name"
│
├── versions/                     # Version history storage
│    ├── documents/               # Regular document versions
│    │   └── path/
│    │       └── to/
│    │           └── doc-name/    # Timestamped version backup
│    │               └── YYYYMMDDhhmmss.md
│    └── pages/                   # Special pages versions
│        └── home/                # Timestamped homepage backup
│            └── YYYYMMDDhhmmss.md
│
├── pages/                        # Special pages (system pages)
│   └── home/                     # Homepage (landing page)
│       └── document.md           # Homepage content
│
├── comments/                     # Document comments
│   └── path/
│       └── to/
│           └── doc-name/         # Timestamped comments for "doc-name"
│               └── YYYYMMDDhhmmss_[user].md
│
└── static/                       # Static assets and customization
    ├── banner.png                # Global banner on all pages (optional, preferred)
    ├── banner.jpg                # Global banner on all pages (optional)
    ├── favicon.ico               # Standard favicon (optional)
    ├── favicon.svg               # SVG format favicon (optional, preferred)
    ├── favicon.png               # PNG format favicon (optional)
    └── langs/                    # Translation files copied by wiki-go at startup
```

扁平文件结构使得备份、版本控制或在应用程序外部操作 Wiki 内容变得容易（如果需要）。 所有内容都存储为纯 Markdown 文件，版本历史记录遵循一个简单的带时间戳的文件命名约定。 文件附件与 document.md 文件存储在同一目录中，从而可以直接管理文档内容及其关联的文件。

---

LeoMoon Wiki-Go 的设计旨在简单部署和使用，同时为知识管理提供强大的功能。 它非常适合团队文档、个人知识库和协作项目。
