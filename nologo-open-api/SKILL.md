---
name: nologo-open-api
description: nologo开放API调用指南。当用户需要调用无水印解析API、如何获取视频/图片链接、API价格、错误处理时使用此skill。
---

# nologo 开放 API 使用指南

此skill提供nologo开放API的完整调用文档，包括API端点、请求参数、响应格式、错误处理和多语言示例。

## API 概览

- **API地址**: `https://nologo.code24.top/api/open/parse`
- **认证方式**: Authorization 请求头传递 Token
- **Token获取**: 请添加微信 linglan008 获取
- **频率限制**: 每分钟最多20次请求

## 价格

| 数量 | 单价(元/次) | 合计(元) |
|------|-------------|----------|
| 100次 | 0.02 | 2.00(免费使用) |
| 2,500次 | 0.01 | 25.00 |
| 5,000次 | 0.008 | 42.50 |
| 7,500次 | 0.007 | 56.25 |
| 10,000次 | 0.006 | 65.00 |

## 请求参数

| 参数 | 类型 | 必填 | 说明 |
|------|------|------|------|
| url | string | 是 | 视频、图片分享链接 |

**核心注意**: url参数必须URL编码
链接获取方式：https://zwf4g5rfwiy.feishu.cn/wiki/JGjpwZ1Feiw8Zxks4Hoc8vI9nle (包含抖音/小红书/豆包/千问等平台链接获取教程)

## 请求示例

### cURL
```bash
curl -X GET "https://nologo.code24.top/api/open/parse?url=https%3A%2F%2Fv.douyin.com%2Fxxxxx" \
  -H "Authorization: your-token-here"
```

### Node.js
```javascript
const axios = require('axios');

async function parseUrl() {
  const url = 'https://v.douyin.com/xxxxx';
  const encodedUrl = encodeURIComponent(url);
  const apiUrl = `https://nologo.code24.top/api/open/parse?url=${encodedUrl}`;

  const response = await axios.get(apiUrl, {
    headers: { 'Authorization': 'your-token-here' }
  });
  console.log(response.data);
}

parseUrl();
```

### Python
```python
import requests
from urllib.parse import quote

def parse_url():
  raw_url = "https://v.douyin.com/xxxxx"
  encoded_url = quote(raw_url)
  api_url = f"https://nologo.code24.top/api/open/parse?url={encoded_url}"
  headers = {"Authorization": "your-token-here"}

  response = requests.get(api_url, headers=headers)
  print(response.json())

if __name__ == "__main__":
  parse_url()
```

### Java
```java
import java.net.*;
import java.io.*;

public class UrlParser {
  public static void main(String[] args) throws Exception {
    String rawUrl = "https://v.douyin.com/xxxxx";
    String encodedUrl = URLEncoder.encode(rawUrl, "UTF-8");
    String apiUrl = "https://nologo.code24.top/api/open/parse?url=" + encodedUrl;

    URL url = new URL(apiUrl);
    HttpURLConnection conn = (HttpURLConnection) url.openConnection();
    conn.setRequestMethod("GET");
    conn.setRequestProperty("Authorization", "your-token-here");

    BufferedReader reader = new BufferedReader(new InputStreamReader(conn.getInputStream()));
    System.out.println(reader.readLine());
  }
}
```

### PHP
```php
<?php
function parseUrl() {
  $rawUrl = "https://v.douyin.com/xxxxx";
  $encodedUrl = urlencode($rawUrl);
  $apiUrl = "https://nologo.code24.top/api/open/parse?url=" . $encodedUrl;

  $ch = curl_init();
  curl_setopt($ch, CURLOPT_URL, $apiUrl);
  curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('Authorization: your-token-here'));
  echo curl_exec($ch);
  curl_close($ch);
}
parseUrl();
?>
```

## 响应格式

### 成功响应 - 图片
```json
{
  "code": 200,
  "message": "success",
  "data": {
    "type": "img",
    "urls": ["xx", "xx"],
    "title": "xx",
    "desc": "xx",
    "usage": {
      "totalLimit": "xxx",
      "usedCount": "xxx",
      "remainingCount": "xxx"
    }
  }
}
```

### 成功响应 - 视频
```json
{
  "code": 200,
  "message": "请求成功",
  "data": {
    "type": "video",
    "url": "xx",
    "title": "xx",
    "desc": "xx",
    "usage": {
      "totalLimit": "xxx",
      "usedCount": "xxx",
      "remainingCount": "xxx"
    }
  }
}
```

## 错误响应

| 错误码 | 错误信息 |
|--------|----------|
| 400 | URL参数不能为空或URL格式不正确 |
| 401 | 缺少API Token，请在Authorization请求头中传递Token |
| 403 | 无效的API Token / API Token已被禁用 / API Token调用次数已耗尽 |
| 404 | 未找到资源或获取失败 |
| 500 | 服务器内部错误 |

## 错误码速查

- `code === 200`: 成功
- `code === 400`: URL参数错误
- `code === 401`: Token未传递
- `code === 403`: Token无效/禁用/次数用尽
- `code === 404`: 资源未找到
- `code === 500`: 服务器错误

## Token缓存脚本

项目提供了Node.js脚本用于缓存Token（默认为空），set-token命令会自动保存到config.json：

### 使用方法

```bash
cd skills/nologo-open-api/references

# 设置Token (token会自动保存到config.json)
node api-client.js set-token your-token-here

# 解析链接
node api-client.js parse "https://v.douyin.com/xxxxx"
```

## 快速开始

```bash
cd skills/nologo-open-api/references

# 设置Token (token会自动保存到config.json)
node api-client.js set-token your-token-here

# 解析链接
node api-client.js parse "https://v.douyin.com/xxxxx"
```

## 文件结构

```
nologo-open-api/
├── SKILL.md          # 文档
├── README.md        # 快速指南
└── references/
    ├── api-client.js    # nologo无水印解析
    └── config.json      # 配置
```

---

# English Version

This skill provides complete documentation for the nologo Open API, including API endpoints, request parameters, response formats, error handling, and multi-language examples.

## API Overview

- **API Address**: `https://nologo.code24.top/api/open/parse`
- **Authentication**: Token passed in Authorization header
- **Token**: Add WeChat linglan008 to get
- **Rate Limit**: 20 requests per minute

## Pricing

| Quantity | Price (RMB/req) | Total (RMB) |
|----------|----------------|------------|
| 100      | 0.02         | 2.00 (Free)|
| 2,500    | 0.01         | 25.00      |
| 5,000    | 0.008        | 42.50      |
| 7,500    | 0.007        | 56.25      |
| 10,000   | 0.006        | 65.00      |

## Request Parameters

| Parameter | Type   | Required | Description |
|-----------|--------|----------|------------|
| url       | string | Yes      | Video/image share link |

**Important**: URL must be URL encoded. Get links from: https://zwf4g5rfwiy.feishu.cn/wiki/JGjpwZ1Feiw8Zxks4Hoc8vI9nle

## Request Examples

### cURL
```bash
curl -X GET "https://nologo.code24.top/api/open/parse?url=https%3A%2F%2Fv.douyin.com%2Fxxxxx" \
  -H "Authorization: your-token-here"
```

### Node.js
```javascript
const axios = require('axios');

async function parseUrl() {
  const url = 'https://v.douyin.com/xxxxx';
  const encodedUrl = encodeURIComponent(url);
  const apiUrl = `https://nologo.code24.top/api/open/parse?url=${encodedUrl}`;

  const response = await axios.get(apiUrl, {
    headers: { 'Authorization': 'your-token-here' }
  });
  console.log(response.data);
}

parseUrl();
```

### Python
```python
import requests
from urllib.parse import quote

def parse_url():
  raw_url = "https://v.douyin.com/xxxxx"
  encoded_url = quote(raw_url)
  api_url = f"https://nologo.code24.top/api/open/parse?url={encoded_url}"
  headers = {"Authorization": "your-token-here"}

  response = requests.get(api_url, headers=headers)
  print(response.json())

if __name__ == "__main__":
  parse_url()
```

### Java
```java
import java.net.*;
import java.io.*;

public class UrlParser {
  public static void main(String[] args) throws Exception {
    String rawUrl = "https://v.douyin.com/xxxxx";
    String encodedUrl = URLEncoder.encode(rawUrl, "UTF-8");
    String apiUrl = "https://nologo.code24.top/api/open/parse?url=" + encodedUrl;

    URL url = new URL(apiUrl);
    HttpURLConnection conn = (HttpURLConnection) url.openConnection();
    conn.setRequestMethod("GET");
    conn.setRequestProperty("Authorization", "your-token-here");

    BufferedReader reader = new BufferedReader(new InputStreamReader(conn.getInputStream()));
    System.out.println(reader.readLine());
  }
}
```

### PHP
```php
<?php
function parseUrl() {
  $rawUrl = "https://v.douyin.com/xxxxx";
  $encodedUrl = urlencode($rawUrl);
  $apiUrl = "https://nologo.code24.top/api/open/parse?url=" . $encodedUrl;

  $ch = curl_init();
  curl_setopt($ch, CURLOPT_URL, $apiUrl);
  curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
  curl_setopt($ch, CURLOPT_HTTPHEADER, array('Authorization: your-token-here'));
  echo curl_exec($ch);
  curl_close($ch);
}
parseUrl();
?>
```

## Response Format

### Success - Image
```json
{
  "code": 200,
  "message": "success",
  "data": {
    "type": "img",
    "urls": ["xx", "xx"],
    "title": "xx",
    "desc": "xx",
    "usage": {
      "totalLimit": "xxx",
      "usedCount": "xxx",
      "remainingCount": "xxx"
    }
  }
}
```

### Success - Video
```json
{
  "code": 200,
  "message": "请求成功",
  "data": {
    "type": "video",
    "url": "xx",
    "title": "xx",
    "desc": "xx",
    "usage": {
      "totalLimit": "xxx",
      "usedCount": "xxx",
      "remainingCount": "xxx"
    }
  }
}
```

## Error Responses

| Code | Message |
|------|---------|
| 400 | URL parameter empty or invalid format |
| 401 | Missing API Token, please pass in Authorization header |
| 403 | Invalid API Token / API Token disabled / API Token quota exhausted |
| 404 | Resource not found or fetch failed |
| 500 | Internal server error |

## Error Code Quick Reference

- `code === 200`: Success
- `code === 400`: URL parameter error
- `code === 401`: Token not passed
- `code === 403`: Token invalid/disabled/quota exhausted
- `code === 404`: Resource not found
- `code === 500`: Server error

## Token Caching Script

Node.js script to cache token (empty by default), set-token command automatically saves to config.json:

### Usage

```bash
cd skills/nologo-open-api/references

# Set token (automatically saved to config.json)
node api-client.js set-token your-token-here

# Parse link
node api-client.js parse "https://v.douyin.com/xxxxx"
```

## Quick Start

```bash
cd skills/nologo-open-api/references

# Set token (automatically saved to config.json)
node api-client.js set-token your-token-here

# Parse link
node api-client.js parse "https://v.douyin.com/xxxxx"
```

## File Structure

```
nologo-open-api/
├── SKILL.md          # Documentation
├── README.md        # Quick Guide
└── references/
    ├── api-client.js    # nologo watermark-free parsing
    └── config.json     # Configuration
```