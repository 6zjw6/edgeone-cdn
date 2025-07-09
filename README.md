# EdgeOne技术教程

> 从入门到实战 · 实时更新  
> 完整版教程：[https://edgeone.zjw6.cn/](https://edgeone.zjw6.cn/)

---

## 教程目录

1. [EdgeOne 简介](#edgeone-简介)
2. [快速入门](#快速入门)
3. [使用案例](#使用案例)
4. [Pages 模板](#pages-模板)
5. [边缘函数](#边缘函数)
6. [安全防护](#安全防护)
7. [集成示例](#集成示例)
8. [工具链配置](#工具链配置)
9. [细节](#细节)

> **点击目录可自动跳转到对应章节**

---

<a name="edgeone-简介"></a>

![EdgeOne 简介](./images/edgeone_intro.png)

## EdgeOne 简介

### 什么是EdgeOne？

EdgeOne是腾讯云推出的下一代边缘安全加速平台，基于腾讯全球3200+边缘节点，提供域名解析、动静态智能加速、TCP/UDP四层加速、DDoS/CC/WAF/Bot防护、边缘函数计算等一体化服务。

- **全球200+ Tbps网络带宽**
- **3200+全球节点覆盖**
- **一站式安全防护**
- **边缘计算能力**

#### 核心架构

用户 → 边缘节点 → 安全防护 → 源站

---

<a name="快速入门"></a>

![快速入门](./images/edgeone_quickstart.png)

## 快速入门

### 接入流程

1. **注册并添加站点**  
   登录EdgeOne控制台，添加您的网站域名

2. **选择接入方式**  
   支持NS接入和CNAME接入两种方式

   - **NS接入**
     - 整个域名托管到EdgeOne
     - 支持所有DNS解析功能
     - 需要修改域名DNS服务器
   - **CNAME接入**
     - 只需添加CNAME记录
     - 快速接入，无需修改DNS
     - 部分功能受限

3. **配置源站**  
   设置您的源站地址和相关参数

   ```yaml
   # 源站配置示例
   源站类型: IP/域名
   源站地址: 192.168.1.100
   端口: 80/443
   回源协议: HTTP/HTTPS
   负载均衡: 轮询
   健康检查: 开启
   ```

4. **DNS解析生效**  
   完成DNS配置后，等待解析生效

   | 解析记录        | CNAME值                          | 生效时间   |
   | --------------- | -------------------------------- | ---------- |
   | api.example.com | api.example.com.eo.dnse5.com     | 2-10分钟   |

### 验证接入效果

#### 性能测试

| 阶段     | 延迟   | 性能提升 |
| -------- | ------ | -------- |
| 接入前   | 729ms  |          |
| 接入后   | 369ms  | 49.4%    |

#### 功能验证

- CDN加速正常
- HTTPS证书有效
- 安全防护开启
- 缓存策略生效

---

<a name="使用案例"></a>

![使用案例](./images/edgeone_cases.png)

## 使用案例

### 电商网站加速

为全球电商平台提供加速和安全防护，提升用户购物体验。

- 商品图片CDN加速
- API接口智能加速
- DDoS攻击防护
- Bot流量管理

**效果展示：**

| 指标         | 优化效果 |
| ------------ | -------- |
| 页面加载速度 | 提升65%  |
| 图片加载时间 | 减少40%  |
| API响应时间  | 优化50%  |

### 视频流媒体平台

为视频平台提供全球加速和智能调度，保证观看体验。

- HLS/DASH自适应
- 视频转码加速
- 防盗链保护
- 带宽成本优化

**核心指标：**

| 指标         | 数值   |
| ------------ | ------ |
| 视频首屏时间 | <2秒   |
| 卡顿率       | <0.5%  |
| 带宽节省     | 30%    |

### 移动应用后端

为移动应用提供API加速和安全防护，提升用户体验。

```js
// 移动端接入示例
const apiUrl = 'https://api.example.com.eo.dnse5.com';
const response = await fetch(`${apiUrl}/user/profile`, {
    method: 'GET',
    headers: {
        'Host': 'api.example.com',
        'Authorization': 'Bearer ' + token
    }
});
```

### 游戏加速服务

为在线游戏提供低延迟加速和DDoS防护。

- UDP加速优化
- 智能路由选择
- 游戏DDoS防护
- 反外挂检测

### 集成代码示例

#### Go语言集成

```go
package main

import (
    "fmt"
    "net/http"
    "io/ioutil"
    "time"
)

func main() {
    start := time.Now()
    body, _ := DoRequest("https://api.example.com.eo.dnse5.com/ping")
    elapsed := time.Since(start)
    fmt.Println("Request with EdgeOne, use", elapsed)
    fmt.Println("Response body:", body)
}

func DoRequest(url string) (string, error) {
    client := &http.Client{}
    request, err := http.NewRequest(http.MethodGet, url, nil)
    if err != nil {
        return "", err
    }
    // 设置Host头
    request.Host = "api.example.com"
    rsp, err := client.Do(request)
    if err != nil {
        return "", err
    }
    defer rsp.Body.Close()
    body, err := ioutil.ReadAll(rsp.Body)
    return string(body), err
}
```

#### JavaScript集成

```js
// 使用EdgeOne加速的API请求
class EdgeOneClient {
    constructor(baseUrl, host) {
        this.baseUrl = baseUrl;
        this.host = host;
    }
    async request(endpoint, options = {}) {
        const url = `${this.baseUrl}${endpoint}`;
        const headers = {
            'Host': this.host,
            'Content-Type': 'application/json',
            ...options.headers
        };
        const response = await fetch(url, {
            ...options,
            headers
        });
        if (!response.ok) {
            throw new Error(`HTTP error! status: ${response.status}`);
        }
        return response.json();
    }
}
// 使用示例
const client = new EdgeOneClient(
    'https://api.example.com.eo.dnse5.com',
    'api.example.com'
);
// 发起请求
const data = await client.request('/user/profile', {
    method: 'GET'
});
```

---

<a name="pages-模板"></a>

![Pages 模板](./images/edgeone_pages.png)

## Pages 模板

快速部署静态网站和前端应用的模板集合。

> EdgeOne Pages是一个类似于Vercel和Cloudflare Pages的静态网站托管平台，支持自动部署、边缘函数和全球加速。

### Next.js 模板

基于Next.js的现代Web应用模板，支持SSR和SSG。

- TypeScript支持
- Tailwind CSS
- 自动部署

```bash
# 快速开始
npx create-next-app@latest my-app
cd my-app
npm run dev

# 部署到EdgeOne Pages
git push origin main
```

### Vue.js 模板

基于Vue 3的单页应用模板，包含路由和状态管理。

- Vue 3 + Vite
- Vue Router
- Pinia状态管理

```bash
# 创建Vue项目
npm create vue@latest my-vue-app
cd my-vue-app
npm install
npm run dev

# 构建部署
npm run build
```

### Angular 模板

企业级Angular应用模板，包含完整的开发工具链。

- Angular CLI
- RxJS
- Material UI

```bash
# 创建Angular项目
ng new my-angular-app
cd my-angular-app
ng serve

# 构建生产版本
ng build --prod
```

### Hexo 博客

静态博客生成器，支持Markdown和主题定制。

- Markdown支持
- 主题丰富
- 插件生态

```bash
# 安装Hexo
npm install -g hexo-cli
hexo init my-blog
cd my-blog
npm install

# 本地预览
hexo server

# 生成静态文件
hexo generate
```

### 文档站点

基于VitePress的技术文档站点模板。

- VitePress
- 全文搜索
- 多语言支持

```bash
# 创建文档站点
npx create-vitepress my-docs
cd my-docs
npm install

# 开发模式
npm run dev

# 构建文档
npm run build
```

### 电商模板

现代电商网站模板，包含完整的购物功能。

- 商品展示
- 购物车
- 支付集成

```bash
# 克隆电商模板
git clone https://github.com/example/ecommerce-template
cd ecommerce-template
npm install

# 配置环境变量
cp .env.example .env
npm run dev
```

### 部署配置

#### GitHub Actions配置

```yaml
# .github/workflows/deploy.yml
name: Deploy to EdgeOne Pages
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
          cache: 'npm'
      - name: Install dependencies
        run: npm ci
      - name: Build
        run: npm run build
      - name: Deploy to EdgeOne Pages
        uses: TencentEdgeOne/pages-action@v1
        with:
          api-token: ${{ secrets.EDGEONE_API_TOKEN }}
          project-name: my-project
          directory: ./dist
```

---

<a name="边缘函数"></a>

![边缘函数](./images/edgeone_functions.png)

## 边缘函数

在边缘节点运行自定义代码，实现高性能计算。

> EdgeOne边缘函数提供Serverless代码执行环境，支持JavaScript/TypeScript，在全球边缘节点运行，延迟极低。

### 基础示例

```js
// 简单的边缘函数示例
export default {
  async fetch(request, env, ctx) {
    // 获取请求信息
    const url = new URL(request.url);
    const userAgent = request.headers.get('User-Agent');
    // 根据用户设备返回不同内容
    if (userAgent.includes('Mobile')) {
      return new Response('Mobile Version', {
        headers: { 'Content-Type': 'text/plain' }
      });
    }
    return new Response('Desktop Version', {
      headers: { 'Content-Type': 'text/plain' }
    });
  }
};
```

### 请求处理

```js
// 处理不同HTTP方法
export default {
  async fetch(request, env, ctx) {
    const url = new URL(request.url);
    const method = request.method;
    switch (method) {
      case 'GET':
        return handleGet(request, url);
      case 'POST':
        return handlePost(request, url);
      case 'PUT':
        return handlePut(request, url);
      default:
        return new Response('Method Not Allowed', {
          status: 405
        });
    }
  }
};

async function handleGet(request, url) {
  // 处理GET请求
  return new Response(JSON.stringify({
    message: 'Hello from EdgeOne',
    timestamp: new Date().toISOString(),
    path: url.pathname
  }), {
    headers: {
      'Content-Type': 'application/json',
      'Cache-Control': 'max-age=300'
    }
  });
}
```

### 高级功能示例

#### 地理位置路由

```js
// 基于地理位置的内容路由
export default {
  async fetch(request, env, ctx) {
    const country = request.cf?.country || 'US';
    const url = new URL(request.url);
    // 根据国家重定向到不同的源站
    const regionMapping = {
      'CN': 'https://china.example.com',
      'US': 'https://us.example.com',
      'EU': 'https://eu.example.com'
    };
    let targetOrigin = regionMapping[country] || regionMapping['US'];
    // 如果是欧盟国家，使用EU服务器
    const euCountries = ['DE', 'FR', 'GB', 'IT', 'ES'];
    if (euCountries.includes(country)) {
      targetOrigin = regionMapping['EU'];
    }
    // 构建新的请求URL
    const newUrl = new URL(url.pathname + url.search, targetOrigin);
    // 发起请求到目标源站
    const response = await fetch(newUrl, {
      method: request.method,
      headers: request.headers,
      body: request.body
    });
    // 添加地理位置信息到响应头
    const newResponse = new Response(response.body, response);
    newResponse.headers.set('X-Country', country);
    newResponse.headers.set('X-Origin', targetOrigin);
    return newResponse;
  }
};
```

#### 身份验证中间件

```js
// JWT身份验证边缘函数
import { verify } from 'jsonwebtoken';
export default {
  async fetch(request, env, ctx) {
    const url = new URL(request.url);
    // 检查是否需要身份验证
    const protectedPaths = ['/api/user', '/api/admin'];
    const needsAuth = protectedPaths.some(path => 
      url.pathname.startsWith(path)
    );
    if (needsAuth) {
      const authResult = await authenticate(request, env);
      if (!authResult.success) {
        return new Response(JSON.stringify({
          error: 'Unauthorized',
          message: authResult.message
        }), {
          status: 401,
          headers: { 'Content-Type': 'application/json' }
        });
      }
      // 将用户信息添加到请求头
      request.headers.set('X-User-ID', authResult.userId);
      request.headers.set('X-User-Role', authResult.role);
    }
    // 继续处理请求
    return fetch(request);
  }
};

async function authenticate(request, env) {
  const authHeader = request.headers.get('Authorization');
  if (!authHeader || !authHeader.startsWith('Bearer ')) {
    return { success: false, message: 'Missing or invalid authorization header' };
  }
  const token = authHeader.substring(7);
  try {
    const payload = verify(token, env.JWT_SECRET);
    return {
      success: true,
      userId: payload.sub,
      role: payload.role
    };
  } catch (error) {
    return { success: false, message: 'Invalid token' };
  }
}
```

#### API限流

```js
// 基于IP的API限流
export default {
  async fetch(request, env, ctx) {
    const clientIP = request.headers.get('CF-Connecting-IP') || 
                    request.headers.get('X-Forwarded-For') || 
                    'unknown';
    // 检查限流
    const rateLimitResult = await checkRateLimit(clientIP, env);
    if (!rateLimitResult.allowed) {
      return new Response(JSON.stringify({
        error: 'Rate limit exceeded',
        retryAfter: rateLimitResult.retryAfter
      }), {
        status: 429,
        headers: {
          'Content-Type': 'application/json',
          'Retry-After': rateLimitResult.retryAfter.toString()
        }
      });
    }
    // 继续处理请求
    const response = await fetch(request);
    // 添加限流信息到响应头
    response.headers.set('X-RateLimit-Limit', '100');
    response.headers.set('X-RateLimit-Remaining', rateLimitResult.remaining.toString());
    response.headers.set('X-RateLimit-Reset', rateLimitResult.resetTime.toString());
    return response;
  }
};

async function checkRateLimit(clientIP, env) {
  const key = `rate_limit:${clientIP}`;
  const windowSize = 60; // 1分钟窗口
  const maxRequests = 100; // 最大请求数
  // 这里可以使用KV存储或其他持久化方案
  // 简化示例使用内存存储
  const now = Math.floor(Date.now() / 1000);
  const windowStart = now - windowSize;
  // 获取当前窗口内的请求数
  const currentCount = await getCurrentCount(key, windowStart);
  if (currentCount >= maxRequests) {
    return {
      allowed: false,
      retryAfter: windowSize - (now % windowSize)
    };
  }
  // 记录当前请求
  await recordRequest(key, now);
  return {
    allowed: true,
    remaining: maxRequests - currentCount - 1,
    resetTime: windowStart + windowSize
  };
}
```

---

<a name="安全防护"></a>

![安全防护](./images/edgeone_security.png)

## 安全防护

全方位的安全防护体系，保护您的业务安全。

### DDoS防护

- 15Tbps防护能力，3秒内响应攻击，自动识别和拦截恶意流量。
- L3/L4/L7层防护
- 智能识别攻击
- 实时监控告警

### Web应用防火墙

- 500+防护规则，防护SQL注入、XSS、CSRF等常见Web攻击。
- OWASP Top10防护
- 自定义规则
- CC攻击防护

### Bot管理

- 智能识别和管理Bot流量，保护API和业务资源。
- 恶意Bot检测
- 合法Bot白名单
- 行为分析

### 安全配置示例

#### WAF配置

```json
// EdgeOne WAF配置示例
{
  "rules": [
    {
      "id": "sql-injection-protection",
      "name": "SQL注入防护",
      "enabled": true,
      "action": "block",
      "conditions": [
        {
          "field": "http.request.uri.query",
          "operator": "contains",
          "value": ["union", "select", "insert", "update", "delete"]
        },
        {
          "field": "http.request.body",
          "operator": "contains", 
          "value": ["'; DROP TABLE", "UNION SELECT", "1=1"]
        }
      ]
    },
    {
      "id": "xss-protection",
      "name": "XSS攻击防护",
      "enabled": true,
      "action": "block",
      "conditions": [
        {
          "field": "http.request.uri.query",
          "operator": "contains",
          "value": ["<script>", "javascript:", "onload="]
        }
      ]
    },
    {
      "id": "custom-rule",
      "name": "自定义防护规则",
      "enabled": true,
      "action": "challenge",
      "conditions": [
        {
          "field": "http.request.uri.path",
          "operator": "equals",
          "value": "/admin"
        },
        {
          "field": "ip.geoip.country",
          "operator": "not_in",
          "value": ["CN", "US", "CA"]
        }
      ]
    }
  ]
}
```

#### 速率限制配置

```json
// 速率限制配置
{
  "rate_limiting": {
    "enabled": true,
    "rules": [
      {
        "id": "api-rate-limit",
        "name": "API限流",
        "match": {
          "request": {
            "uri": {
              "path": {
                "starts_with": "/api/"
              }
            }
          }
        },
        "threshold": 100,
        "period": 60,
        "action": "block",
        "identifier": "ip.src"
      },
      {
        "id": "login-rate-limit", 
        "name": "登录限流",
        "match": {
          "request": {
            "uri": {
              "path": {
                "equals": "/login"
              }
            },
            "method": "POST"
          }
        },
        "threshold": 5,
        "period": 300,
        "action": "challenge",
        "identifier": "ip.src"
      }
    ]
  }
}
```

#### Bot管理配置

```json
// Bot管理配置
{
  "bot_management": {
    "enabled": true,
    "detection_mode": "intelligent",
    "rules": [
      {
        "id": "good-bots",
        "name": "允许的Bot",
        "action": "allow",
        "bot_categories": [
          "search_engine",
          "social_media",
          "monitoring"
        ],
        "user_agents": [
          "Googlebot",
          "Bingbot",
          "FacebookBot",
          "TwitterBot"
        ]
      },
      {
        "id": "bad-bots",
        "name": "恶意Bot",
        "action": "block",
        "bot_categories": [
          "spam",
          "scraper",
          "ddos"
        ],
        "signatures": [
          "scrapy",
          "python-requests",
          "curl"
        ]
      },
      {
        "id": "suspicious-bots",
        "name": "可疑Bot",
        "action": "challenge",
        "conditions": [
          {
            "field": "http.request.headers.user_agent",
            "operator": "not_contains",
            "value": "Mozilla"
          },
          {
            "field": "http.request.headers.accept",
            "operator": "not_contains",
            "value": "text/html"
          }
        ]
      }
    ]
  }
}
```

---

<a name="集成示例"></a>

![集成示例](https://via.placeholder.com/800x200?text=集成示例)

## 集成示例

### Node.js 集成

```js
// 使用EdgeOne加速API的Node.js示例
const fetch = require('node-fetch');
(async () => {
  const response = await fetch('https://api.example.com.eo.dnse5.com/user', {
    headers: { 'Host': 'api.example.com' }
  });
  const data = await response.json();
  console.log(data);
})();
```

### Python 集成

```python
# 使用EdgeOne加速API的Python示例
import requests
headers = {'Host': 'api.example.com'}
r = requests.get('https://api.example.com.eo.dnse5.com/user', headers=headers)
print(r.json())
```

### Java 集成

```java
// 使用EdgeOne加速API的Java示例
import java.net.*;
import java.io.*;
public class EdgeOneDemo {
  public static void main(String[] args) throws Exception {
    URL url = new URL("https://api.example.com.eo.dnse5.com/user");
    HttpURLConnection conn = (HttpURLConnection) url.openConnection();
    conn.setRequestProperty("Host", "api.example.com");
    BufferedReader in = new BufferedReader(new InputStreamReader(conn.getInputStream()));
    String inputLine;
    while ((inputLine = in.readLine()) != null) System.out.println(inputLine);
    in.close();
  }
}
```

### 集成常见问题（FAQ）

- **Q: 如何设置自定义Host头？**
  > A: 在请求时通过 `Host` 头部指定源站域名，见上方代码示例。
- **Q: 支持哪些语言SDK？**
  > A: 支持所有可自定义HTTP头的语言，官方推荐Node.js、Python、Go、Java等。
- **Q: API限流如何处理？**
  > A: 可结合EdgeOne限流规则和应用层自定义限流。

---

<a name="工具链配置"></a>

![工具链配置](https://via.placeholder.com/800x200?text=工具链配置)

## 工具链配置

### 推荐工具链

| 工具         | 说明                 | 官网链接                                  |
| ------------ | -------------------- | ----------------------------------------- |
| Node.js      | 服务端JS运行环境     | [nodejs.org](https://nodejs.org/)         |
| Python       | 脚本与自动化         | [python.org](https://www.python.org/)     |
| Go           | 高性能后端开发        | [golang.org](https://golang.org/)         |
| GitHub Actions | 自动化CI/CD         | [github.com/actions](https://github.com/features/actions) |
| Vercel       | 前端部署与托管        | [vercel.com](https://vercel.com/)         |
| Docker       | 容器化部署           | [docker.com](https://www.docker.com/)     |

### 工具链配置示例

```yaml
# GitHub Actions自动部署EdgeOne Pages
name: Deploy to EdgeOne Pages
on:
  push:
    branches: [main]
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: '18'
      - name: Install dependencies
        run: npm ci
      - name: Build
        run: npm run build
      - name: Deploy to EdgeOne Pages
        uses: TencentEdgeOne/pages-action@v1
        with:
          api-token: ${{ secrets.EDGEONE_API_TOKEN }}
          project-name: my-project
          directory: ./dist
```

### 工具链FAQ

- **Q: 如何本地调试EdgeOne函数？**
  > A: 推荐使用 [Miniflare](https://miniflare.dev/) 或 [Wrangler](https://developers.cloudflare.com/workers/wrangler/) 进行本地模拟。
- **Q: CI/CD失败如何排查？**
  > A: 检查API Token权限、构建日志、依赖安装等常见问题。

---

<a name="细节"></a>

![细节](https://via.placeholder.com/800x200?text=细节)

## 细节

### 性能优化

- 启用全站HTTPS，提升安全性与SEO。
- 静态资源使用CDN缓存，减少源站压力。
- 合理配置缓存策略，动态与静态分离。
- 使用HTTP/2、Gzip压缩提升传输效率。

### 安全防护

- 配置WAF规则，防护SQL注入、XSS等攻击。
- 启用DDoS防护，定期检查安全日志。
- 对敏感API启用速率限制和Bot管理。

### 监控与告警

- 配置实时监控，关注流量、延迟、错误率。
- 设置自动告警，及时响应异常。
- 定期回顾和优化安全策略。

### FAQ

- **Q: 如何快速定位性能瓶颈？**
  > A: 结合EdgeOne监控与源站日志，分析延迟与流量分布。
- **Q: 如何应对突发流量？**
  > A: 启用自动扩容和DDoS防护，合理配置缓存。

### 参考资料

- [EdgeOne官方文档](https://edgeone.zjw6.cn/)
- [腾讯云文档中心](https://cloud.tencent.com/document/product/1552)





---

## 贡献指南

欢迎提交PR、Issue或建议！

1. Fork本仓库
2. 新建分支并提交修改
3. 提交PR并描述变更内容

如有疑问请联系 [zjw@zjw6.cn](mailto:zjw@zjw6.cn)

---

## 致谢

感谢EdgeOne和社区成员的支持！

---

## 版本说明

- v1.0 初版发布，涵盖EdgeOne核心功能与最佳实践
- v1.1 增加多语言集成示例与FAQ
- v1.2 全面美化文档结构与排版

> 文档持续更新中，欢迎关注与反馈！
    
    
