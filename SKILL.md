---
name: minimax-h3-prompts
description: 为本地 ComfyUI 上的 MiniMax H3 音视频模型写、改、查提示词——以参考生视频（Ref2VA / R2V：人物图+场景图+音频）为主，也覆盖文生视频 T2VA、首帧生视频 I2VA、首尾帧 FL2VA。按官方 H3-Context-IR 的英文分段结构输出（subject_definitions / summary / retention_analysis / detailed_description / overall_soundscape / non_diegetic_music），处理多人多参考、唱跳与对白口型、分镜切换、运镜，以及「油感、AI感、贴图感、开场先给全景、人物串脸串装」等问题。用户提到 H3、MiniMax 视频、R2V、参考生视频、给工作站写视频提示词、出片发油发假、多参考效果差时使用。不用于 Seedance、可灵、MJ 等其他模型。
---

# MiniMax H3 提示词

## 先知道这一点

H3 开源的只是生成模型本身。官方线上服务在生成前，还有一个没开源的改写系统（H3-Context-IR），它先把用户的需求改写成一套固定的英文分段格式，再交给模型。官方原话是这一步「对最终质量至关重要」。

本地 ComfyUI 没有这一步，提示词原样进模型。所以本地写提示词，就是**自己扮演这个改写系统**：照官方格式写英文、分段、编号。中文 markdown 或自由格式的提示词离模型训练时见过的格式太远，更容易出现油感、指令不听、参考图乱用。

## 流程

1. **判断模式**

   | 模式 | 本地工作流 | 素材 |
   |---|---|---|
   | Ref2VA 参考生视频 | `06-参考生视频-R2V-*` | 最多 9 张图、3 段视频、3 段音频 |
   | T2VA 文生视频 | `04-文生视频-*` | 只有文字 |
   | I2VA 首帧生视频 | `05-图生视频-*` | 1 张图当第 1 帧 |
   | FL2VA / L2VA 首尾帧 | FL2VA 模型 | 首帧和/或尾帧 |

2. **清点素材、定分工**：按接入节点的顺序给素材编号，图片、视频、音频各自从 1 开始数（`<Picture 1>`、`<Video 1>`、`<Audio 1>`）。每份素材先写清楚**它负责什么、不负责什么**，详见 [参考控制](references/reference-control.md)。这一步决定了会不会贴图感、会不会开场先给全景。

3. **按格式写提示词**：Ref2VA 写六段，其他模式写三段。格式细节读 [官方格式要点](references/official-format.md)，完整示例读 [唱跳 MV 示例](references/example-kpop-solo.md)。

4. **逐项自检**（见下方清单）。

5. **交付**：
   - 英文提示词单独放一个代码块，可以直接粘贴进节点。
   - 代码块外用中文写几句：每张图接到第几个口、这条提示词的镜头安排。
   - 附上参数建议（分辨率、步数、参考图尺寸），读 [本地参数与取舍](references/comfyui-local.md)。

## 硬规则

- **正文用英文**。台词、歌词、画面里出现的文字保留原语言，写在 `<d>[Korean] ...</d>` 里，不翻译。
- **人物、场景、服装都定义成 `<Subject N>`**，在定义句里写明来自哪张 `<Picture N>`。只提供人物或场景的图，不单独给它开一行 `<Picture N>` 条目；只有当图本身就是某一帧（首帧、尾帧、关键帧）时才单列。
- **先写一句风格**，再写 `[Shot 1]`。例如 `The target video is a live-action, cinematic K-pop music video ...`。
- **镜头编号**：`[Shot 1]` 不带时间；后面每个镜头写 `[Shot N] At 00:02.500, the shot cuts to ...`，时间严格递增，而且都在总时长以内。
- **运镜写成完整的英文句子**：运镜类型 + 幅度 + 速度，例如 `The camera pushes in with small amplitude at fast speed`。不要在句末堆标签。
- **会出声的人都给编号** `(S1)`、`(S2)`，一起唱写 `(S1,S2)`；第一次出现时写清音色、语速、性别年龄。音色描述和动作写在 `<d>` 外面。
- **`detailed_description` 写 350–500 个英文词**。每个镜头都写清楚：景别、人物在画面哪里、在做什么、光从哪来、镜头怎么动、此刻有什么声音。
- **开场景别要写死**：`[Shot 1]` 第一句就写景别和人物正在做的动作，并加一句 `The video begins directly on this framing; it does not open on a wide establishing view or recreate the composition of any reference picture.` **例外**：某张图本身就是这一条的首帧 / 开场构图（比如 MJ 分镜图），就写 `<Picture N> is the opening composition of this shot`，去掉 `or recreate the composition of any reference picture` 这半句，只保留不从全景开场。
- **人物要被场景的光照到**：每个镜头至少写一处人物和环境的光影关系，比如光从哪边打来、墙面的颜色反射到皮肤上、脚下的接触阴影、背景虚化程度。这是防贴图感的关键。
- **不写空洞的质量词**，比如 `masterpiece`、`8K`、`best quality`、`ultra detailed`。

## 自检清单

- [ ] 接入节点的每张图都被引用了，没有引用不存在的编号。
- [ ] 每个 `<Subject N>` 都在 subject_definitions 里定义过，在 retention_analysis 里各有一行。
- [ ] 每份参考素材都写了「负责什么、不负责什么」，尤其是三视图设定图的灰底、平光、多格排版、站姿。
- [ ] `[Shot 1]` 第一句就是近景或特写（如果要求如此），而且写了「不从参考图构图开场」（有专门的首帧 / 开场构图图时例外：写明它就是开场构图）。
- [ ] 时间戳递增；最后一个镜头的开始时间小于总时长；歌词句数和时长对得上。
- [ ] 每句歌词或台词前都写了是谁 `(Sx)`；多人时，每句只有一个人张嘴，除非写明是合唱。
- [ ] `detailed_description` 在 350–500 词左右；除 `<d>` 内的歌词和画面文字外，没有中文。
- [ ] 每个镜头都写了光影关系。
- [ ] 近景镜头里没写地面、鞋子、"full body"这类会把镜头拉远的东西。
- [ ] 群众写了在干什么、看哪里；走路写了朝向和方向；要正常速度时写了正常速度。
- [ ] 接原曲的话，这段 ≤ 15 秒（最好 5 秒），要唱在推近之后的那句落在第 2~3 秒。
- [ ] overall_soundscape 只写环境声和动作声，不写歌词；non_diegetic_music 写乐器、速度、强弱变化，不写情绪形容词。

## 说明

- **MV 批量实战（2026-10-28~30，《Red Line》一百多条）**：首帧图怎么给、近景写地面会拉远、动作时间偏晚、群众表演、俯拍、一采不精修、种子怎么挑等规律，见 [MV 实战记录](references/mv-field-notes.md)。给 MV 写提示词前先读这份。

- **本工作站实测（2026-10-28）**：A 独唱·粉色宫殿，LightX2V 6 步，1280×736，2 个种子，旧写法和新格式对比。结论：
  - 新格式的脸更像真人：皮肤有纹理、光影对比更明显，旧写法偏「磨皮娃娃脸」。两个种子都是这样。
  - 开场的全身镜头从约 2.5 秒以上缩短到约 0.5–1 秒，但还没完全消除。改进办法见 [参考控制](references/reference-control.md) 第 2 节。
  - 参考图尺寸 `max` 和 `match` 差别不明显，`max` 每段多用约 36% 的时间（212 秒对 156 秒）。
- 资料来源：官方仓库 [MiniMax-AI/MiniMax-H3](https://github.com/MiniMax-AI/MiniMax-H3) 的 `skills/h3-prompt-writing`（写作规范），以及社区实测 [LoveRain1997/h3-prompt-journal](https://github.com/LoveRain1997/h3-prompt-journal)（MIT 许可）、[awesome-minimax-H3](https://github.com/wildminder/awesome-minimax-H3) 性能报告、[matsuo-koya/minimax-h3-notes](https://github.com/matsuo-koya/minimax-h3-notes)（RTX 5090 实测）。
