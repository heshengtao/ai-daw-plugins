# AI DAW 扩展市场索引

这是 [AI DAW](https://github.com/heshengtao/ai-daw) 的**扩展插件**（应用级插件：MCP 工具、右栏面板、内容包）的商店仓库。
应用默认从这里读 `index.json`：

```
https://raw.githubusercontent.com/heshengtao/ai-daw-plugins/main/index.json
```

> 这里的「扩展」不是 VST3 / CLAP 音频插件。扩展是 npm 包，装进 daemon 之后能给 AI 加工具、
> 给界面加面板；音频插件仍然走原来的插件管理器。

## 目前状态

索引是**空的**（`plugins: []`），等第一批扩展发布后再加。空索引是合法状态：应用的「发现」页
会显示空态，不会报错。

## 怎么把一个扩展加进来

1. 扩展作者发一个 npm 包，包里有 `aidaw.json`（`package.json` 的 `aidaw.manifest` 指向它），
   并声明 `engines.aidaw`（例如 `">=1.9.0 <2.0.0"`）。格式见
   [扩展开发指南](https://github.com/heshengtao/ai-daw/blob/main/skills/aidaw/references/extensions.md)。
2. 把包名加进 `plugins.txt`（一行一个，`#` 开头是注释）。
3. 在 [ai-daw](https://github.com/heshengtao/ai-daw) 仓库根目录跑生成器：

   ```sh
   node tools/build-extension-index.mjs \
     --input ../ai-daw-plugins/plugins.txt \
     --output ../ai-daw-plugins/index.json \
     --title "AI DAW 扩展" --description "社区扩展"
   ```

   生成器只读 npm registry 元数据（不下载、不执行包代码），没有声明 `engines.aidaw` 的版本会被跳过。
4. 提交并推送 `index.json`。应用下一次刷新（缓存 10 分钟）就能看到。

## 格式

`index.json` 是 `format: 1`，字段与 DeepSeek Harness 的插件市场索引一致，只把 `engines.dsh`
换成 `engines.aidaw`。完整说明见
[商店索引文档](https://github.com/heshengtao/ai-daw/blob/main/docs/extensions-index.md)。

## 安全

索引**没有签名**：标题、作者、标签都是市场自述，不代表任何验证。应用会显示索引地址与安装来源，
并且在安装前要求用户确认——装扩展等于在这台机器上执行第三方代码。发现可疑条目请开 issue。

## 许可

索引数据以 MIT 发布；各扩展的许可见其 npm 包。
