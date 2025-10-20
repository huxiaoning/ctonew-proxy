# 相关连接

- [[2api]cto.new的，没办法了只能拿来高质量翻译了 - 开发调优 / 开发调优, Lv2 - LINUX DO](https://linux.do/t/topic/1056491)
- [Ctonew2api - 福利羊毛 / 福利羊毛, Lv3 - LINUX DO](https://linux.do/t/topic/1056745)
- [cto-2api指南 - 开发调优 / 开发调优, Lv1 - LINUX DO](https://linux.do/t/topic/1056750)



# Deno部署

本项目用于在 Deno Deploy 上部署一个可兼容 OpenAI/Claude 格式的 API 代理服务，支持流式返回、身份验证与跨域访问。
整个流程无需服务器、无需数据库，一键部署即可使用。

## 0️⃣ 准备一个Ctonew账号

- 注册或登录 [cto.new | Home](https://cto.new/)
- 然后获取`Cookie`中的`__client`的值.
- 最终的API Key即为`__client=<__client的值>`



## 1️⃣ [fork](https://github.com/yourname/enginelabs-deno-proxy/fork) 本项目

点击上方链接 fork 当前仓库到你自己的 GitHub 账户中。
 后续 Deno Deploy 会从你的仓库自动拉取部署。

## 2️⃣ 登录 / 注册 Deno Deploy

访问 https://dash.deno.com/
 使用 GitHub 账户登录或注册。

## 3️⃣ 创建新项目

进入 https://dash.deno.com/new_project
 点击 **“+ New Project”** 创建一个新的 Deno 服务。

快速部署点击:[![Deploy to Deno](https://deno.com/deploy.svg)](https://console.deno.com/mir-xiong/~/new)



## 4️⃣ 选择你的 GitHub 仓库

在弹出的仓库列表中选择你刚刚 fork 的项目。
 填写项目名称（非常重要，会影响自动生成的访问域名，例如 `yourname.deno.dev`）。



## 5️⃣ 填写项目配置

在 **Entrypoint** 一栏填写：`server.ts`



## 6️⃣ 点击 <b>Deploy Project</b>

Deno Deploy 会自动：

- 拉取你的仓库代码
- 安装依赖（若有）
- 启动 `server.ts` 服务

整个过程通常只需几秒钟。



## 7️⃣ 部署成功 ✅

部署完成后页面会显示一个分配的域名，例如：

```
https://your-project-name.deno.dev/
```

该域名即可直接访问，例如浏览器打开`https://your-project-name.deno.dev/v1/models`将看到：

```
{
  "object": "list",
  "data": [
    {
      "id": "ClaudeSonnet4_5",
      "object": "model",
      "created": 1234567890,
      "owned_by": "enginelabs"
    },
    {
      "id": "GPT5",
      "object": "model",
      "created": 1234567890,
      "owned_by": "enginelabs"
    }
  ]
}
```

## 8️⃣ 作为 Chat API 代理使用

你也可以将该域名用作标准的 Chat API 代理，例如：

```shell
curl https://your-project-name.deno.dev/v1/chat/completions \
  -H "Authorization: Bearer <your_enginelabs_api_key>" \
  -H "Content-Type: application/json" \
  -d '{
    "model": "ClaudeSonnet4_5",
    "messages": [
      {"role": "user", "content": "你好，介绍一下你自己"}
    ],
    "stream": false
  }'
```

返回结果与 OpenAI API 完全兼容，可直接在前端或 SDK 中使用。

------

## 🌈 额外功能

- ✅ SSE 流式响应
- ✅ CORS 支持
- ✅ 请求透传
- ✅ 无需服务器运维
- ✅ 可自定义模型映射

------

## 🧠 提示

如需自定义模型映射、添加验证或日志，可直接修改 `server.ts` 中的对应逻辑。
 修改后只需在 Deno Deploy 面板中点击 **“Redeploy”** 即可重新发布。
