使用方法
启动本地服务（新版 llama.cpp 自带 HTTP 服务）：

bash


llama-server -m 你的模型.gguf --port 8080 --ctx-size 4096
浏览器打开 translate.html，默认地址 http://127.0.0.1:8080，点「检测连接」看到绿点即成功。

功能要点
两种接口模式：默认 OpenAI 兼容 /v1/chat/completions（自动回退旧版 /chat/completions），也可切到 llama.cpp 内置 /completion；流式响应三种格式（SSE、裸文本、JSON）都能正确解析
翻译质量保障：内置严格的 system prompt（只出译文、保留格式、术语一致），支持附加要求、temperature / top_p / max_tokens 调节
流式逐字输出：带闪烁光标与进度条，可随时「停止生成」（Esc 亦可）
15 种语言 + 自动检测、一键交换、复制、历史记录（localStorage，点击可回填）
状态面板：显示模型名与延迟；出错时给出可操作提示（含启动命令）
所有文本只在你本机浏览器和 llama.cpp 之间传输
如果访问被 CORS 拦截（个别编译版本），启动时加 --api-key 无关，加 --no-webui 也无关——直接确认服务端是近期版本即可，默认已允许跨域。

新增功能说明
统一汇入：四种来源（直接输入 / 文件 / 图片 / 网页）解析出的文本都追加到左侧文本框，可编辑、可手动加批注，再按 Ctrl + Enter 走本地 LLM 翻译。长文本会自动上调 max_tokens。

Table


来源	实现	支持格式 / 说明
📄 文件	pdf.js 逐页解析、mammoth 解析 docx，均纯浏览器端	.pdf、.docx；多文件合并；旧版 .doc 需另存为 .docx
🖼 图片	Tesseract.js（WASM）本地 OCR，可选识别语言	清晰横排文字效果最佳；中文建议选"简体+英文"；带缩略图预览
🔗 网页	自动尝试 4 种跨域方式提取正文	经公共代理抓取，超长仅取前 6000 字，可勾选"抓取后自动翻译"
几点提醒

文件/图片/网页功能首次使用需能访问 jsDelivr（加载解析库与 OCR 语言包）；翻译推理始终在你的本地 llama.cpp 完成。若完全离线，可下载 pdf.min.js / pdf.worker.min.js / mammoth.browser.min.js / tesseract.min.js 放本地并把脚本地址改掉。
OCR 中文识别率有限，竖排、艺术字、模糊图建议先预处理或换更清晰图。
网页翻译很长时，注意本地模型 --ctx-size 要够大，否则会在终端条报"未返回内容"。
网页抓取依赖公共代理，个别强反爬/登录墙页面会失败，属正常现象。
