
# 如何在 Elastic Cloud 上使用 Web Crawler 爬取私有网络

## 概述

Elastic 的 Web Crawler 可以通过 HTTP 代理访问私有网络中的内容，从而爬取非公共环境中的受保护资源。本文将通过一个详细的示例，说明如何配置 Web Crawler 来实现这一目标。

如需高质量的代理服务，推荐使用 [Proxy-Seller](https://bit.ly/proxy-seller-coupon)。它提供超过 2000 万个住宅 IP，覆盖全球 197 个国家，支持精确的城市定位和 ISP 级别的匿名性，非常适合需要长期稳定运行的爬取项目。

---

## 基础架构概览

Web Crawler 被托管在 Elastic Cloud 上，但它可以访问私有网络中的资源。以下是一个示例架构图，用于展示如何使用认证代理服务器实现此功能：

![Crawler Proxy Schematic](https://www.elastic.co/guide/en/enterprise-search/current/images/crawler-proxy-schematic.png)

### 核心组件

- **Web Crawler**：支持 HTTP 和 HTTPS 协议。建议配置 TLS 来保护内容和代理服务器凭证。
- **代理服务器**：作为中介，允许 Web Crawler 通过代理访问受保护的网络资源。

> 有关代理服务器的详细设置，请参考您的代理软件文档。

---

## 测试代理连接

在配置 Enterprise Search 部署以使用 HTTP 代理之前，请先测试代理连接，确保其能够正常访问私有网站。

### 测试步骤

1. 配置您的浏览器使用代理，然后尝试访问目标私有网站。
2. 或者，使用以下命令通过代理访问目标网站的主页：

   ```bash
   curl --head --proxy https://proxyuser:proxypass@proxy.example.com http://marlin-docs.internal
   ```

3. 如果连接成功，您将收到类似以下的响应：

   ```plaintext
   HTTP/1.1 200 OK
   Content-Type: text/html
   Content-Length: 42337
   Server: nginx/1.14.2
   X-Cache: HIT from test-proxy
   Connection: keep-alive
   ```

---

## 配置 Enterprise Search

为了使 Web Crawler 的所有操作都通过 HTTP 代理进行，需要在 Enterprise Search 部署中添加以下用户设置：

```yaml
connector.crawler.http.proxy.host: proxy.example.com
connector.crawler.http.proxy.port: 3128
connector.crawler.http.proxy.protocol: https
connector.crawler.http.proxy.username: proxyuser
connector.crawler.http.proxy.password: proxypass
```

### 配置指南

1. 将上述设置添加到您的部署配置文件中。
2. 部署完成后，系统将自动进行平滑重启。
3. 详细配置步骤请参考 [Elastic 官方文档](https://www.elastic.co/guide/en/cloud/current/ec-manage-enterprise-search-settings.html)。

> 使用带有基本身份验证的 HTTP 代理需要适当的 Elastic 订阅级别。有关订阅详情，请访问 [Elastic Cloud](https://www.elastic.co/subscriptions/cloud) 或 [自托管部署页面](https://www.elastic.co/subscriptions)。

---

## 测试配置

配置完成后，按照以下步骤验证爬取功能是否正常：

1. 在 Kibana 的 **Search** 页面创建一个 Elasticsearch 索引。
2. 选择 **Use the Web Crawler** 作为数据采集方式。
3. 添加目标域名 URL，并进行验证。

验证过程如下图所示：

![Crawler Proxy Validation](https://www.elastic.co/guide/en/enterprise-search/current/images/crawler-proxy-validation.png)

如果未出现验证警告，请检查您的部署配置，确保代理已正确配置。

---

## 爬取内容

一旦私有域名成功添加到配置中，您即可开始爬取内容。爬取的内容将被自动存储到您的 Elastic 部署中。如果遇到问题，请检查以下日志：

- **Crawler 日志**：[查看 Web Crawler 事件日志](https://www.elastic.co/guide/en/enterprise-search/current/crawler-view-events-logs.html)。
- **代理服务器日志**：检查代理是否正确处理了请求。

---

## 推荐代理服务

为确保爬取的高效性和稳定性，使用优质代理服务非常重要。推荐使用 [Proxy-Seller](https://bit.ly/proxy-seller-coupon)。它提供以下优势：

- **全球覆盖**：超过 2000 万个 IP，涵盖 197 个国家。
- **高稳定性**：99.99% 的正常运行时间，适合长时间稳定使用。
- **精确定位**：支持城市和 ISP 级别的精准地理定位。

立即访问 [Proxy-Seller](https://bit.ly/proxy-seller-coupon)，获取最佳代理服务，优化您的爬取体验！

---
