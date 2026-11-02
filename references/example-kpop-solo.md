# 示例：单人唱跳 MV（参考生视频，10 秒）

## 素材和接线

| 接线口 | 素材 | 负责 | 不负责 |
|---|---|---|---|
| ref_image_0 → `<Picture 1>` | 成员 A 三视图设定图 | 脸、发型、妆容、配饰、整套服装 | 灰底、棚拍平光、多格排版、站姿 |
| ref_image_1 → `<Picture 2>` | 粉色巴洛克大堂（裁成楼梯和吊灯那一块） | 建筑、材质、颜色、光源（斜射的阳光） | 空旷大全景的构图、开场画面 |

参数：1280×736，243 帧（10 秒），`ref_image_size = max`。

## 镜头安排（145 BPM，每 2.5 秒一个镜头，切在拍上）

1. 近景：甩发、起势，开唱第一句；镜头快速推近。
2. 特写：眨眼比心；镜头小幅弧线移动。
3. 低机位近景：靠在金色栏杆上，卡点指向镜头；镜头横移。
4. 近景转特写：转一圈后托腮；镜头快速推近，最后半秒静止。

## 提示词

```text
subject_definitions:
<Subject 1> is the young female K-pop performer from <Picture 1>, a character turnaround sheet: her face comes from the large close-up panel on the left, and her outfit from the front, back and side panels. She has long straight black hair with thin side braids, a large white satin bow and small butterfly clips, soft pink eye makeup and glossy rose lips; she wears an off-shoulder pink open-knit cardigan over a white crop top, a pearl necklace with a silver heart pendant, a light-blue striped shirt tied around her waist, a cream pleated mini skirt with a lace hem, pink arm warmers, white over-knee socks and white platform Mary Jane shoes. Only her facial identity, hairstyle, makeup, accessories and costume are taken from <Picture 1>; the sheet's gray studio backdrop, flat studio lighting, multi-panel layout, masked faces and neutral standing pose are not used.
<Subject 2> is the pink baroque palace hall from <Picture 2>: pink marble walls with white ornamental plasterwork, twin curved grand staircases with gold railings, crystal chandeliers, a glossy pink-and-white checkerboard marble floor, tufted pink velvet sofas and palms in gold pots, with hazy sunbeams slanting in from tall windows. <Picture 2> provides only the architecture, materials, colors and light sources; its wide, empty, centered composition is not an opening frame and is not recreated.

summary:
[reference generation] A ten-second K-pop rap performance in which <Subject 1> (S1) raps fast Korean-English lines while dancing inside <Subject 2>. The video is built from four beat-cut shots, almost entirely medium close-ups and close-ups, with the palace visible only as a soft, sunlit background.

retention_analysis:
<Subject 1> (appears in [Shot 1], [Shot 2], [Shot 3], [Shot 4]): fully_preserved - her face, hair, bow, makeup and full costume are retained; she is relit by the palace's sunbeams and pink bounce light instead of the sheet's studio lighting.
<Subject 2> (appears in [Shot 1], [Shot 2], [Shot 3], [Shot 4]): partially_preserved - the pink marble, white plasterwork, gold railings, chandeliers, checkerboard floor and slanting sunbeams are retained as a shallow-focus background behind close framing; the wide overview of the hall is never shown.

detailed_description:
The target video is a live-action, cinematic K-pop music video with a symmetrical pastel palette, natural skin texture with visible pores and fine 35mm film grain.
[Shot 1] A medium close-up from chest height frames <Subject 1> at the center of the frame; the video begins directly on this framing and does not open on a wide establishing view or recreate the composition of any reference picture. She is already mid-bounce on the first kick drum, standing in a hazy sunbeam that falls on her left cheek and shoulder while the pink marble walls bounce a soft pink fill onto her skin; the gold railings and chandeliers behind her are soft and out of focus. She sweeps her long hair behind her shoulder so the white bow swings, and a young woman with a bright, playful, fast and crisp rapping voice (S1) raps: <d>[Korean] Pink light, 내 맘은 already high!</d> The camera pushes in with small amplitude at fast speed, landing on the downbeat.
[Shot 2] At 00:02.500, the shot cuts to a close-up of her face, eyes level with the lens. She winks and presses a finger heart beside her cheek as she (S1) raps: <d>[Korean] 계단 위로 step by step, 따라와 봐!</d> Her lips move fast and precisely with every syllable. The camera arcs right with small amplitude at slow speed, and the crystal chandeliers turn into warm round bokeh over her shoulder.
[Shot 3] At 00:05.000, the shot cuts to a low-angle medium close-up: the camera sits at hip height about one meter in front of her, tilted up. She leans her shoulder against the gold railing at the foot of the staircase, sunlight rimming her hair, and on each snare hit she points her index finger at the lens while she (S1) raps: <d>[Korean] Sweet but I'm fast, 놓치지 마!</d> The camera trucks right with small amplitude at fast speed, following her.
[Shot 4] At 00:07.500, the shot cuts to a medium close-up as she spins one full turn in place, the pleated skirt and the tied shirt sleeves flaring outward and her hair whipping across the frame, then stops facing the lens. She (S1) raps the last line: <d>[Korean] 오늘 밤 주인공은 나, that's right!</d> The camera pushes in at fast speed to a close-up as she rests her chin on both fists and smiles; for the final half second the camera holds a static shot.

overall_soundscape:
The quick tap of platform shoes on marble, the rustle of knit fabric and a pleated skirt, and the soft natural echo of a large hall.

non_diegetic_music:
A fast trap-pop beat at about 145 BPM with deep 808 bass, rapid hi-hat rolls, crisp hand claps and bright synth stabs, driving at constant intensity to a hard stop on the final downbeat.
```

## 实测结果（2026-10-28，6 步，1280×736，2 个种子）

- 脸比旧写法更自然：有皮肤纹理，光影对比也更明显。
- 4 个镜头基本都按设计出来了：比心特写、靠栏杆指镜头、转圈后托腮。
- 开头仍有约 0.5–1 秒的全身镜头。改法见 [参考控制](reference-control.md) 第 2 节：第 1 个镜头不提地面，也不用推近。
- 比心的特写镜头里，眨眼常被做成两只眼都使劲眯起来，表情偏夸张。想要更收敛的效果，可以写 `a quick, subtle wink with one eye`。

## 和旧写法的区别

| 旧写法（第 2、3 版） | 新写法 |
|---|---|
| 中文 markdown，标题、加粗、列表 | 官方英文六段格式 |
| 直接写 `<Picture 1>` 指人物 | 用 `<Subject 1>` 指人物，在定义里绑定 `<Picture 1>` |
| 只说「不要全景开场」 | 写明每张图负责什么、不负责什么，第一镜第一句直接给近景和正在做的动作 |
| 没写人物和环境的光照关系 | 每个镜头都写光从哪来、反射色、虚化 |
| 歌词写在「」里，按时间列表 | 歌词写在 `<d>[Korean] ...</d>`，嵌在对应镜头里，带 `(S1)` |
| 运镜用中文描述 | 官方运镜词：类型 + 幅度 + 速度，再补上机位高度和距离 |
