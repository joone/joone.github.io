---
title: 'Enhancing Puppeteer with Proxy Support'
date: 2021-08-22T13:48:00.002-07:00
author: "Joone Hur"
categories: ["headless"]
draft: false
aliases: [ "/2021/08/proxy-support-puppeteer.html" ]
tags : [puppeteer,chromium]
images:
  - "images/post/puppeteer_logo.png"
---
Puppeteer is a popular Node.js library for controlling headless Chrome. 
It allows developers to automate tasks such as web scraping and testing. 
However, until recently, Puppeteer did not support proxy settings for each BrowserContext. 
This meant that if you wanted to use a proxy with Puppeteer, you had to set it globally for all BrowserContexts.
You were not able to set a diffrent proxy for each indivisual BrowseContext at runtime.

To address this limitation, I made a change to the Chromium codebase as follows:
```C++
diff --git a/content/browser/devtools/protocol/target_handler.cc b/content/browser/devtools/protocol/target_handler.cc
index 7a30ab12a1a9f..ce66dc10a49cf 100644
--- a/content/browser/devtools/protocol/target_handler.cc
+++ b/content/browser/devtools/protocol/target_handler.cc
@@ -810,14 +810,20 @@ void TargetHandler::DevToolsAgentHostCrashed(DevToolsAgentHost* host,
 // ----------------- More protocol methods -------------------
 
 protocol::Response TargetHandler::CreateBrowserContext(
+    Maybe<std::string> proxy_url,
     std::string* out_context_id) {
+  std::string proxyURL;
+  if (proxy_url.isJust()) { 
+    proxyURL = proxy_url.fromJust();
+  }
   if (access_mode_ != AccessMode::kBrowser)
     return Response::Error(kNotAllowedError);
   DevToolsManagerDelegate* delegate =
       DevToolsManager::GetInstance()->delegate();
   if (!delegate)
-    return Response::Error("Browser context management is not supported.");
-  BrowserContext* context = delegate->CreateBrowserContext();
+    return Response::Error(
+        "Browser context management is not supported.");
+  BrowserContext* context = delegate->CreateBrowserContext(proxyURL);
   if (!context)
     return Response::Error("Failed to create browser context.");
   *out_context_id = context->UniqueId();
diff --git a/content/browser/devtools/protocol/target_handler.h b/content/browser/devtools/protocol/target_handler.h
index ea0dd51cfb03c..fbb3625585935 100644
--- a/content/browser/devtools/protocol/target_handler.h
+++ b/content/browser/devtools/protocol/target_handler.h
@@ -84,7 +84,8 @@ class TargetHandler : public DevToolsDomainHandler,
                        bool* out_success) override;
   Response ExposeDevToolsProtocol(const std::string& target_id,
                                   Maybe<std::string> binding_name) override;
-  Response CreateBrowserContext(std::string* out_context_id) override;
+  Response CreateBrowserContext(Maybe<std::string> proxy_url,
+                                std::string* out_context_id) override;
   void DisposeBrowserContext(
       const std::string& context_id,
       std::unique_ptr<DisposeBrowserContextCallback> callback) override;
diff --git a/content/public/browser/devtools_manager_delegate.cc b/content/public/browser/devtools_manager_delegate.cc
index f313b48c869ee..d74ef223a0103 100644
--- a/content/public/browser/devtools_manager_delegate.cc
+++ b/content/public/browser/devtools_manager_delegate.cc
@@ -45,7 +45,8 @@ BrowserContext* DevToolsManagerDelegate::GetDefaultBrowserContext() {
   return nullptr;
 }
 
-BrowserContext* DevToolsManagerDelegate::CreateBrowserContext() {
+BrowserContext* DevToolsManagerDelegate::CreateBrowserContext(
+    const std::string& proxyUrl) {
   return nullptr;
 }
 
diff --git a/content/public/browser/devtools_manager_delegate.h b/content/public/browser/devtools_manager_delegate.h
index 4b5ba76385c35..2307977c5abfa 100644
--- a/content/public/browser/devtools_manager_delegate.h
+++ b/content/public/browser/devtools_manager_delegate.h
@@ -52,7 +52,7 @@ class CONTENT_EXPORT DevToolsManagerDelegate {
   // Create new browser context. May return null if not supported or not
   // possible. Delegate must take ownership of the created browser context, and
   // may destroy it at will.
-  virtual BrowserContext* CreateBrowserContext();
+  virtual BrowserContext* CreateBrowserContext(const std::string& proxyUrl);
 
   // Dispose browser context that was created with |CreateBrowserContext|
   // method.
diff --git a/headless/lib/browser/headless_devtools_manager_delegate.cc b/headless/lib/browser/headless_devtools_manager_delegate.cc
index fa2f1143c2786..5f42bc03707ea 100644
--- a/headless/lib/browser/headless_devtools_manager_delegate.cc
+++ b/headless/lib/browser/headless_devtools_manager_delegate.cc
@@ -86,9 +86,17 @@ HeadlessDevToolsManagerDelegate::GetDefaultBrowserContext() {
 }
 
 content::BrowserContext*
-HeadlessDevToolsManagerDelegate::CreateBrowserContext() {
+HeadlessDevToolsManagerDelegate::CreateBrowserContext(
+    const std::string& proxyURL) {
   auto builder = browser_->CreateBrowserContextBuilder();
   builder.SetIncognitoMode(true);
+
+  if (!proxyURL.empty()) {
+    std::unique_ptr<net::ProxyConfig> proxy_config =
+        std::make_unique<net::ProxyConfig>();
+    proxy_config->proxy_rules().ParseFromString(proxyURL);
+    builder.SetProxyConfig(std::move(proxy_config));
+  }
   HeadlessBrowserContext* browser_context = builder.Build();
   return HeadlessBrowserContextImpl::From(browser_context);
 }
diff --git a/headless/lib/browser/headless_devtools_manager_delegate.h b/headless/lib/browser/headless_devtools_manager_delegate.h
index 9a43363d3f023..640d1daf3172d 100644
--- a/headless/lib/browser/headless_devtools_manager_delegate.h
+++ b/headless/lib/browser/headless_devtools_manager_delegate.h
@@ -45,7 +45,8 @@ class HeadlessDevToolsManagerDelegate
 
   std::vector<content::BrowserContext*> GetBrowserContexts() override;
   content::BrowserContext* GetDefaultBrowserContext() override;
-  content::BrowserContext* CreateBrowserContext() override;
+  content::BrowserContext* CreateBrowserContext(
+      const std::string& proxyURL) override;
   void DisposeBrowserContext(content::BrowserContext* context,
                              DisposeCallback callback) override;
 
diff --git a/third_party/blink/public/devtools_protocol/browser_protocol-1.3.json b/third_party/blink/public/devtools_protocol/browser_protocol-1.3.json
index 764c46f5b1893..f71c51d3ebf69 100644
--- a/third_party/blink/public/devtools_protocol/browser_protocol-1.3.json
+++ b/third_party/blink/public/devtools_protocol/browser_protocol-1.3.json
@@ -3938,6 +3938,9 @@
             {
                 "name": "createBrowserContext",
                 "description": "Creates a new empty BrowserContext. Similar to an incognito profile but you can have more than one.",
+                "parameters": [
+                    { "name": "proxyUrl", "type": "string", "description": "The proxy URL including the port.", "optional": true }
+                ],
                 "returns": [
                     { "name": "browserContextId", "$ref": "BrowserContextID", "description": "The id of the context created." }
                 ],
diff --git a/third_party/blink/public/devtools_protocol/browser_protocol.pdl b/third_party/blink/public/devtools_protocol/browser_protocol.pdl
index 3228ff45670cb..cb88540a8dbe0 100644
--- a/third_party/blink/public/devtools_protocol/browser_protocol.pdl
+++ b/third_party/blink/public/devtools_protocol/browser_protocol.pdl
@@ -6667,6 +6667,9 @@ domain Target
   # Creates a new empty BrowserContext. Similar to an incognito profile but you can have more than
   # one.
   experimental command createBrowserContext
+    parameters
+      # The proxy URL.
+      optional string proxyUrl
     returns
       # The id of the context created.
       Browser.BrowserContextID browserContextId

```
The modifications involved passing a proxy server address to HeadlessBrowserContext during the creation of a BrowserContext.  
Additionally, adjustments were made to the Puppeteer interface, specifically in the Browser::createIncognitoBrowserContext function as follows:
```JS
async createIncognitoBrowserContext(
    options: BrowserContextOptions = {}
  ): Promise<BrowserContext> {
    const { proxyServer = '', proxyBypassList = [] } = options;

    const { browserContextId } = await this._connection.send(
      'Target.createBrowserContext',
      {
        proxyServer,
        proxyBypassList: proxyBypassList && proxyBypassList.join(','),
      }
    );
    const context = new BrowserContext(
      this._connection,
      this,
      browserContextId
    );
    this._contexts.set(browserContextId, context);
    return context;
  }

```
At that time, I was quite busy with my project, so I did not submit the changes upstream. Eventually, Pavel Feldman, a Google engineer, implemented proxy support for each BrowseContext and merged his patch to upstream.

https://chromium-review.googlesource.com/c/chromium/src/+/2226298

Despite of this proxy support, the createIncognitoBrowserContext function within Puppeteer remained unchanged.
So I recently made a change to Puppeteer that allows developers to set proxy settings for each BrowserContext:
https://github.com/puppeteer/puppeteer/pull/7516


### How to set a proxy for indivisual browse context

We can now set a diffrent proxy for each indivisual BrowseContext at runtime.
```JS
"use strict";

const puppeteer = require("puppeteer");

(async () => {
  const browser = await puppeteer.launch();
  const context = await browser.createIncognitoBrowserContext(
    {proxyServer: "http://yourproxy.server"}
  );
  const page = await context.newPage();
  await page.authenticate({ username: "your_id", password: "your_password" });
  await page.goto("https://google.com");
  console.log(await page.content());
  await browser.close();
})();

```

Bofore, we were only able to apply a global proxy setting for all all BrowserContexts. 
```JS
  const browser = await puppeteer.launch({
    args: [
      `--proxy-server=proxy-server.your_proxy.server:8001`,
      "--proxy-bypass-list=*",
    ],
  });
  const page = await browser.newPage();
  await page.authenticate({
    username: "your_id",
    password: "your_passwd",
  });

  await page.goto("http://www.google.com");
  console.log(await page.content());
  await browser.close();

```

For more details on the API, see the Puppeteer documentation: https://pptr.dev/api/puppeteer.browsercontextoptions





