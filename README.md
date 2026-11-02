# MiniMax H3 提示词（Claude Code skill）

给 Claude Code 用的 skill：为本地 ComfyUI 上的 MiniMax H3 音视频模型写、改、查提示词。以参考生视频（Ref2VA / R2V：人物图 + 场景图 + 音频）为主，也覆盖文生视频 T2VA、首帧生视频 I2VA、首尾帧 FL2VA。按官方 H3-Context-IR 的英文分段结构输出，处理多人多参考、唱跳与对白口型、分镜切换、运镜，以及油感、AI 感、贴图感、开场先给全景、人物串脸串装等问题。

## 包含什么

| 文件 | 内容 |
|---|---|
| `SKILL.md` | 流程、硬规则、自检清单 |
| `references/official-format.md` | 官方格式要点 |
| `references/reference-control.md` | 多张参考图怎么分工、开场不照搬参考图、防油感和贴图感、多人锚定 |
| `references/comfyui-local.md` | 本地参数与取舍（RTX 5090 上的实测） |
| `references/example-kpop-solo.md` | 唱跳 MV 完整示例 |
| `references/mv-field-notes.md` | 一支 AI MV 批量实战（一百多条）总结的规律：首帧图、近景别写地面、动作时间偏晚、群众表演、俯拍、一采不精修、种子怎么挑 |

## 安装

把整个文件夹放到 `~/.claude/skills/minimax-h3-prompts/`（Windows 是 `C:\Users\你的用户名\.claude\skills\minimax-h3-prompts\`），不要只复制 `SKILL.md`。

## 资料来源

官方仓库 [MiniMax-AI/MiniMax-H3](https://github.com/MiniMax-AI/MiniMax-H3) 的 `skills/h3-prompt-writing`，社区实测 [LoveRain1997/h3-prompt-journal](https://github.com/LoveRain1997/h3-prompt-journal)（MIT）、[awesome-minimax-H3](https://github.com/wildminder/awesome-minimax-H3)、[matsuo-koya/minimax-h3-notes](https://github.com/matsuo-koya/minimax-h3-notes)，以及作者本机工作站的实测。
