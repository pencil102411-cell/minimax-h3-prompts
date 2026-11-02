# 参考控制：多张参考图怎么分工

人物图、场景图分开给，模型会面临三个问题：
- 人物要长成哪张图的样子？
- 光照用哪张图的？
- 第一帧要不要照着哪张图摆？

不写清楚，它就会自己猜。常见的猜错结果有三种：
- 把三视图里的灰底和棚拍平光带进视频，人物像贴上去的。
- 第一帧照搬场景大全景，或者照搬设定图的站姿。
- 多人时脸和衣服互相串。

下面的做法来自社区实测（主要是 [h3-prompt-journal](https://github.com/LoveRain1997/h3-prompt-journal) 的案例 019、021、024，MIT 许可），以及我们在本工作站上的出片观察。

## 1. 每张图都写「负责什么 / 不负责什么」

参考图会被模型当成「整体真相」：图里有什么，它都倾向于搬进视频。只说「别照抄姿势」不管用，要给每张图写一份**授权清单**和一份**禁止清单**。

在官方六段格式里，这两份清单写在 `subject_definitions` 的定义句里，再在 `retention_analysis` 里呼应一次。

人物设定图（三视图）的写法：

```text
<Subject 1> is the performer from <Picture 1>, a character turnaround sheet: her face comes from the large close-up panel on the left, and her outfit from the front, back and side panels. Only her facial identity, hairstyle, makeup, accessories and costume are taken from <Picture 1>. The sheet's gray studio backdrop, flat studio lighting, multi-panel layout, masked faces and neutral standing pose are not used.
```

场景图的写法：

```text
<Subject 2> is the location from <Picture 2>: ... <Picture 2> provides only the architecture, materials, colors and light sources of the location; its wide, empty, centered composition is not an opening frame and is not recreated.
```

retention_analysis 里呼应：

```text
<Subject 1> (...): fully_preserved - identity, hair, makeup and full costume are retained; she is relit by the location's light instead of the sheet's studio lighting.
<Subject 2> (...): partially_preserved - materials, colors and light sources are retained as a shallow-focus background behind close framing; the wide overview is not shown.
```

## 2. 开场不从参考图开始

问题：生成的视频开头会先摆出参考图的样子。比如场景图是空的大全景，开头就先来 1–2 秒全景甚至空镜；人物图是站姿，开头就先站定，然后才开始跳。

做法：
- 在 `[Shot 1]` 第一句写明景别和人物**已经在做**的动作，比如 `already mid-bounce on the first kick drum`。
- 紧跟一句：`The video begins directly on this framing; it does not open on a wide establishing view or recreate the composition of any reference picture.`
- 把「开场姿势由谁决定」交给文字里写好的第一个舞蹈动作：`Begin directly with the first choreographed movement described below.` 只写「不要照抄图」会留下空缺，模型会用参考图来填。
- 场景图尽量用**和成片景别接近的局部**。要拍近景，就给楼梯和吊灯那一块，不要给整个大厅的空全景。
- **例外**：如果给了一张专门的首帧 / 开场构图图（例如 MJ 分镜图），它就是用来照搬开场构图的：写 `<Picture N> is the opening composition of this shot`；"不照搬构图"只对设定图、场景图写。见 [MV 实战记录](mv-field-notes.md)。

本工作站的实测效果：
- 只在中文提示词里写「景别要求」并裁剪场景图，开头仍然先给约 2–3 秒全身镜头。
- 换成官方格式加上面这些写法后，缩短到约 0.5–1 秒，但还在。

剩下这一小段全身镜头，是第 1 个镜头的写法招来的（2026-10-29《Red Line》批量实测已验证，见 [MV 实战记录](mv-field-notes.md)）：
- 提到了 `checkerboard floor`，地面要入画，景别自然会放宽。
- 写了 `the camera pushes in`，模型会从更远的位置开始推。

做法：第 1 个镜头不提地面、鞋子这类只有全身镜头才看得到的东西，并写 `The first frame does not show the floor; the camera never pulls back to a wide view.`；运镜改成 `holds a handheld close framing`，推近留到第 2 个镜头再用。另外给一张已经是目标景别的首帧图当参考，开场会直接照它的景别。

## 3. 防油感、AI感、贴图感

贴图感的本质是：人物和环境的光照不统一。改法分两部分。

**提示词里**，每个镜头都把人物「放进」环境的光里：
- 光从哪边来，落在脸和衣服的哪里，例如 `warm sunbeam from the high window on her left cheek`。
- 环境颜色反射到皮肤上，例如 `pink bounce light from the marble walls on her skin`。
- 接触关系，例如 `her platform shoes cast short contact shadows on the checkerboard floor`、`she leans her shoulder against the gold railing`。
- 背景虚化要和景别对得上：近景时背景要虚。
- 写实的质感词只写一次，写在开头的风格句里：`natural skin texture with visible pores and fine film grain`。不要在每个镜头里反复堆「realistic」。

**素材上**：
- 三视图设定图可以拆开用：脸部特写裁成一张、正面全身裁成一张，分别作为两张参考图接入。不过《Red Line》实测四栏设定图（脸部特写 + 正背侧无脸全身）整张挂也没问题：衣服配饰全对、不会出无脸的人。先整张挂，出问题再拆。
- 场景图不能有人、不能有黑边，最好是自然光、写实材质的图。CG 感太重的场景图会把整段视频带向 CG。
- 参考图尺寸选 `max`（官方默认会把参考图短边放到 2048），不要用 `match`。`match` 会把参考图缩到和生成画面一样小，脸的细节会丢。详见 [本地参数](comfyui-local.md)。

## 4. 多人：写一个「身份锚定块」，每次原样粘贴

社区做四人群舞的经验：
- **只写一次**每个人的锚定描述：在画面中的位置、发型、完整服装清单。之后每一段提示词都一字不改地粘贴同一段。正文里只用 `<Subject 1>`...`<Subject 4>` 指代，不再重复描述外貌。这样模型就不会每段重新「设计」一遍角色。
- **用位置区分人**：`<Subject 1> stands far left, <Subject 2> center-left ...`。
- **换队形是跳着换的**，不是走过去：写 `the supporting foot pivots first, knees and torso follow, the head turns last`；每个人只在自己的区域里用半步调整，不穿过别人身前。
- **用切镜代替拉远**：新队形通过切到一个已经站好位的近景来展示。镜头一拉远，舞蹈就变成小人在动。
- **每个镜头只给 1–2 个人近景**更稳。我们实测：四人同框时，衣服会串，比如 B 的手上出现了 A 的粉色袖套。
- 明确写人数：`Exactly four performers; no extra dancer, clone or background figure.`

## 5. 音乐时间轴驱动一切

- 先定 BPM，换算出一拍、一小节有多长。例如 145 BPM：一拍约 0.41 秒，一小节约 1.66 秒。
- **切点卡在拍上**，动作的峰值落在重拍上。镜头时间戳尽量取拍长的整数倍。
- 没有现成音乐时，`non_diegetic_music` 要写得很具体：曲风、BPM、乐器、哪里有重音。正文里也要写明「动作跟着这段节拍走」。
- **切在动作进行到一半的地方**：上一镜头的动作还没做完就切，下一个镜头接着做。不要「摆好姿势、切、再摆新姿势」，那样会像幻灯片。

## 6. 分段做长视频时

- 每段都要能独立成立：每段都完整重贴锚定块，时间从 00:00 重新计。
- 段尾写一句「出画状态」（重心在哪只脚、手臂在往哪甩），下一段开头写一句「入画状态」从这里接上。

## 7. 运镜：写机位的物理位置

社区的体会：只写效果名，比如 `low angle`，模型会简化。最好同时写明机位的物理位置和距离，例如：

```text
the camera sits at hip height about one meter in front of her, tilted up, and pushes in with small amplitude at fast speed
```

运镜类型照样用官方的那套词（见 [官方格式要点](official-format.md)），再补上机位高度、距离、相对人物的方位。
