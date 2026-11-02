# 官方格式要点

整理自官方仓库 `skills/h3-prompt-writing` 的两份写作指南（`base-en.txt` 管文生、首帧、首尾帧；`ref-en.txt` 管参考生视频）。HuggingFace 上 `docs/VIDEO_PROMPT_WRITING_GUIDE_*_en.md` 是同一份内容。下面是中文要点，加上必须照抄的格式片段；完整原文见官方仓库。

## 一、参考生视频（Ref2VA）：六段，顺序固定

```text
subject_definitions:
...

summary:
...

retention_analysis:
...

detailed_description:
...

overall_soundscape:
...

non_diegetic_music:
...
```

### 1. subject_definitions：给素材起标签

一共四种标签：

| 标签 | 用途 |
|---|---|
| `<Subject N>` | 从素材里抽出来、要在视频里出现的东西：人、动物、物件、场景、服装、道具、风格、动作 |
| `<Picture N>` | 图片本身就是某一帧：首帧、关键帧、尾帧、构图锚点、分镜参考 |
| `<Video N>` | 整段视频的关系：被编辑的原片、要续写的原片、借用的运镜或剪辑节奏 |
| `<Audio N>` | 音频：直接复用，或参考音色、节奏、曲风 |

规则：
- 每样东西单独一行，说清它是什么、扮演什么角色、要跟随的主要特征。
- 只用来定义人物或场景的图，**不单列**，而是写进对应 `<Subject N>` 的定义里，比如 `<Subject 1> is the young woman in <Picture 1>, with ...`。
- 一个 Subject 可以来自多份素材，要写明每份素材提供什么，比如长相来自 `<Picture 1>`、动作来自 `<Video 1>`。
- 音频绑定到某个说话人时写：`<Audio 1> is the voice-timbre reference for <Subject 1> (S1).`
- 标签一旦定下来，六段里的意思都不能变。

### 2. summary：一小段英文总述，开头用方括号写任务类型

| 任务类型 | 什么时候用 |
|---|---|
| `keyframe completion` | 图片当首帧、关键帧、尾帧 |
| `reference generation` | 图片、视频、音频只提供人物、场景、风格、动作、运镜等参考（**唱跳 MV 一般是这个**） |
| `video editing` | 直接修改一段原视频 |
| `video continuation` | 接着原视频往下续 |
| `audio reuse` | 原样复用一段音频 |
| `audio reference` | 只参考音色、节奏、曲风，不复用原声 |

多种关系用 ` + ` 连接，比如 `[reference generation + audio reuse]`。summary 里不能出现新标签。

### 3. retention_analysis：每个标签一行，写它保留到什么程度

画面类标签用：
- `fully_preserved`：完整保留
- `partially_preserved`：保留一部分，或者改了一部分特征
- `attribute_transfer`：特征转移到另一个主体身上
- `weak_reference`：只保留大致的风格、类别、构图、氛围

音频标签用：`fully_copy`、`partially_copy`、`reference`、`weak_reference`。

写法：

```text
<Subject 1> (appears in [Shot 1], [Shot 3]): fully_preserved - ...
<Audio 1>: reference - ...
```

这一段**不写** `(Sx)` 说话人编号。新加的动作、背景、剧情不算「保留得不好」。

### 4. detailed_description：正文，按播放顺序逐镜头写

- 先用一两句英文定风格，再写 `[Shot 1]`。
- `[Shot 1]` 不带时间；后续镜头写 `[Shot N] At MM:SS.mmm, the shot cuts to ...`。
- 标签第一次出现时，写它在这个镜头里能看到的特征、在画面哪里、在做什么；后面的镜头直接用标签，不用重新定义。
- 图片当首帧或尾帧时这样写：`the shot begins from <Picture 1>` / `the shot ends on <Picture 3>`。
- 人物说话或唱歌时，同时写人物标签和说话人编号：`<Subject 2> (S1) ...`。
- 长度：生成类任务一般 **350–500 个英文词**；对白很多时，以把完整台词时间线写下为准。即使只有一个镜头，也不要写短。

### 5 和 6. overall_soundscape 和 non_diegetic_music

- `overall_soundscape`：1–4 句，写整段视频的环境声、动作声、呼吸笑声这类非语言人声。台词和歌词不写在这里。
- `non_diegetic_music`：1–3 句，写只有观众听得到的配乐：乐器、速度、节奏、强弱变化。不写「感人」「紧张」这类情绪词。没有配乐就写 `N/A`。
- 完整的台词和歌词只写在 `detailed_description` 的 `<d>` 里，这两段不重复。

## 二、文生 / 首帧 / 首尾帧：三段

```text
integrated_multimodal_description: [Shot 1] ...

overall_soundscape: ...

non_diegetic_music: ...
```

有图片时，第一行先写一句对齐说明，空一行后再写三段：
- **首帧 I2VA**：`For the target video, at 0.00 seconds into the target video, <Picture 1> (from [Shot 1]) is fully referenced.`
- **首尾帧 FL2VA**：`How the reference pictures align with the target video — Picture 1 (from Shot 1) aligns with the 0.00-second mark of the target video; Picture 2 (from Shot N) aligns with the S.SS-second mark of the target video.`
- **只有尾帧 L2VA**：`How the reference pictures align with the target video — <Picture 1> (from [Shot N]) aligns with the S.SS-second mark of the target video.`

`S.SS` 是总时长，保留两位小数。首尾帧任务尽量写成一个镜头，让模型连续地从首帧过渡到尾帧。

`[Shot 1]` 的开头要写风格和初始构图，比如 `[Shot 1] Live-action, cinematic, a medium-wide shot frames ...`。

## 三、两种模式通用的写法

### 切镜

后面的镜头用 `the camera cuts to` / `the shot cuts to` / `the shot transitions to` 等。每次切镜都要带来新信息：新的主体、空间、状态、视角或时间。如果只是景别稍微变一下，用运镜，不要切。

### 运镜：类型 + 幅度 + 速度

| 类型 | 意思 |
|---|---|
| `Zoom In / Zoom Out` | 变焦，机身不动 |
| `Push In / Pull Out` | 机身前进 / 后退 |
| `Pan Left / Pan Right` | 机身不动，镜头左右转 |
| `Truck Left / Truck Right` | 机身左右平移 |
| `Tilt Up / Tilt Down` | 机身不动，镜头上下转 |
| `Pedestal Up / Pedestal Down` | 机身整体升 / 降 |
| `Arc Shot` | 绕着主体走弧线 |
| `Tracking Shot` | 跟着运动的主体走 |
| `Static Shot` | 完全不动 |
| `Shake Slightly / Shake Strongly` | 轻微 / 强烈晃动 |
| `POV` | 主观视角 |
| `Roll Clockwise / Counterclockwise` | 绕镜头轴旋转 |

幅度写 `with small amplitude` / `with large amplitude`，速度写 `at slow speed` / `at fast speed`。幅度中等、速度正常时可以省略。要写成自然句子：`The camera arcs left with small amplitude at slow speed around her.`

### 说话人、对白、唱词

- 会出声的人用固定编号 `(S1)`、`(S2)`，换镜头也不变；从不出声的人不给编号。
- 第一次出现时写清身份和声音特征：性别、年龄感、音高、音色、语速、口音。
- `<d>` 里只放语言标签和原话，原样保留每个字和标点：`The young woman with a quiet, breathy voice (S1) says: <d>[English] I get off at the next station.</d>`
- 画外音写 `says in an off-screen voiceover`，并且紧跟一句「画面里这个人嘴唇是闭着的」。
- 一句话跨过切镜时，在两边都用 `<scenetrans>`，并写明 `continues seamlessly across the cut`。话被视频结尾截断时用 `<cutoff>`。

### 画面里的文字

招牌、字幕、霓虹字放在英文双引号里，原文照抄：`A red neon sign reading "营业中" glows above the doorway.`

## 四、这次整理时发现、官方没写明的地方

- **中英混唱的语言标签**：官方没有给出例子。目前用的是按主体语言写一个标签，例如 `<d>[Korean] Pink light, 내 맘은 already high!</d>`。效果未验证。另据社区测试，加速 LoRA 会让中英混唱、口音变得平淡。
- **ComfyUI 节点**：`MiniMaxH3ReferenceToVideo` 节点识别 `<Picture N>`、`<Video N>`、`<Audio N>`，编号按接线顺序。`<Subject N>` 是写给模型理解的语义标签，不对应接线口。
