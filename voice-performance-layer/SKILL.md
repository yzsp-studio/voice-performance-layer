---
name: voice-performance-layer
description: |
  AI 视频/音频"声音表演"精准控制专家。基于副语言学（Paralanguage）理论，
  解决 AI 配音"平、假、没有情绪层次"的核心问题。
  人类语音中，实际"说了什么"只占沟通信息量的 7%（Mehrabian 法则），
  38% 来自"怎么说的"——语速、音调、停顿、气息、声音质感——这就是副语言。
  本 Skill 将语言学和声乐表演学的副语言体系转化为 AI TTS 和 Seedance 视频
  可理解的声音表演指令，覆盖六大维度：
  1. 语速控制（Tempo）——快慢缓急的情绪映射
  2. 音高变化（Pitch）——音调起伏的表意功能
  3. 音量动态（Volume）——响度变化的权力与情感信号
  4. 停顿设计（Pause）——沉默的表演力量
  5. 声音质感（Voice Quality）——气声/颤声/沙哑/鼻音的情绪编码
  6. 非言语发声（Non-verbal Vocalizations）——叹气/哼声/吞咽/笑声的精确描述
  与「video-prompt-craft」互补：video-prompt-craft = 视觉表演层（微表情/眼神/肌肉链），
  本 Skill = 听觉表演层（声音/停顿/气息/副语言）。
  两者叠加 = 视听同步的完整角色真实感。
  触发词包括：声音表演、语气控制、paralanguage、副语言、说话方式、
  配音指导、voice acting、voice direction、语速控制、音调变化、
  停顿设计、气声、颤声、声音质感、voice quality、
  叹气声、哼声、清嗓、非言语发声、vocal expression、
  让配音更自然、配音太平、TTS 没有感情、让声音有情绪、
  speech performance、vocal control、tone of voice、
  情绪语音、台词指导、dialogue direction。
summary-cn: 声音表演指令层——副语言精准控制
summary-en: Voice performance layer — precise paralinguistic control
version: 1.0.0
tags: [Audio, Video, Prompt Engineering, Paralanguage, Voice Acting, TTS, Speech Performance]
tags-cn: [视频, 音频, 配音, 声音表演]
trigger-words: [声音表演, 语气控制, paralanguage, 副语言, 说话方式, 配音指导, voice acting, voice direction, 语速控制, 音调变化, 停顿设计, 气声, 颤声, 声音质感, voice quality, 叹气, 哼声, 非言语发声, vocal expression, 让配音更自然, 配音太平, TTS没有感情, 让声音有情绪, speech performance, vocal control, tone of voice, 台词指导, dialogue direction]
author: yzsp-ai
repository: https://github.com/yzsp-ai
---


# 声音表演指令层 — Voice Performance Layer

让 AI 的声音从"读字机器"变成"有灵魂的表演者"。

## 铁律（写声音指令前必读）

1. **每段台词至少标注 2 个声音维度**，全部平铺 = 和没标一样
2. **声音质感必须匹配当前情绪**，不能角色在哭但声音质感写"清澈明亮"
3. **停顿不是空白**：每个停顿必须搭配非言语发声填充（吞咽/呼吸/叹气）
4. **角色声音基线先定**：不定基线就写指令 = 每段台词的声音人设都在飘
5. **视听情绪一致**：声音指令和视觉指令（表情/动作）的情绪方向必须对齐

## 反模式

| 反模式 | 翻车原因 | 正确做法 |
|--------|---------|---------|
| 只标注"语气：悲伤" | TTS 不理解抽象情绪 | 标注具体维度：语速减至 2字/s + 音高降至最低 + 气声质感 |
| 所有台词同一个语速 | 听起来像新闻播报 | 句内变速：关键词前减速、情绪爆发处加速 |
| 停顿写成空白沉默 | 听起来像音频卡顿 | 停顿 + `[deep breath]` 或 `[swallow]` |
| 多角色但声音基线不写 | 听不出谁在说话 | 每个角色先建基线四维（音域/语速/音量/质感） |
| 声音写"愤怒"但画面写"微笑" | 视听矛盾 | 声音和视觉指令统一：都指向同一情绪 |

**核心理念**：同一句台词"我没事"，可以是：
- 轻快上扬 + 正常语速 = 真的没事
- 气声 + 降速 50% + 2s 停顿后说 = 强忍泪水
- 音量突降 + 声音发紧 = 压抑愤怒
- 鼻音加重 + 尾音下坠 = 刚哭完

**怎么说**比**说什么**更重要。

**与其他 Skill 的关系**：
- `video-prompt-craft` = **看**到的表演（微表情/眼神/肌肉链/运镜）→ 视频提示词生成
- `character-body-language` = 无意识的身体泄漏（体态/空间/步态/微动作）→ 角色真实感
- **本 Skill** = **听**到的表演（语速/音调/停顿/气息/声音质感）+ 无意识的声音泄漏（颤音/吞咽/清嗓）

---

## STEP 0: DIAGNOSE — 声音哪里"不对"

| 症状 | 诊断 | 调用模块 |
|------|------|---------|
| 配音太平，像读稿 | 缺少音高变化和停顿 | → 音高 + 停顿 |
| 情绪和台词不匹配 | 声音质感和情绪矛盾 | → 声音质感 §情绪映射 |
| 没有呼吸感 | 缺少非言语发声 | → 非言语发声 §呼吸标记 |
| 角色性格不鲜明 | 缺少个性化语音基线 | → STEP 1 角色声音基线 |
| 对话节奏拖沓 | 停顿位置不对 | → 停顿设计 |
| 紧张场景声音太淡定 | 缺少语速和音量的情绪修正 | → 语速 + 音量 |

---

## STEP 1: CHARACTER VOICE BASELINE — 角色声音基线

在写任何台词的声音指令前，先定义角色的"默认声音"。这是情绪偏移的基准。

### 声音基线四维

| 维度 | 低端 | 中位 | 高端 |
|------|------|------|------|
| 音域 | 低沉（男低音/女中音） | 中等 | 明亮（男高音/女高音） |
| 语速 | 慢（2-3 字/s） | 中（4-5 字/s） | 快（6-8 字/s） |
| 音量 | 轻柔 | 正常对话 | 洪亮 |
| 质感 | 沙哑/磁性 | 清澈 | 尖锐/金属 |

### 身份-声音映射

| 角色类型 | 音域 | 语速 | 音量 | 质感 | 示例描述 |
|---------|------|------|------|------|---------|
| 权威长者 | 低 | 慢 | 中偏低 | 沙哑磁性 | `deep baritone, measured pace of 3 words/s, gravelly texture` |
| 年轻女主 | 中高 | 中快 | 中 | 清澈 | `bright mid-range, lively 5 words/s, clear and bell-like` |
| 反派 | 低 | 慢（刻意） | 低到高剧变 | 丝滑/阴冷 | `silky low register, deliberately slow, volume erupting unpredictably` |
| 儿童 | 高 | 快且不规则 | 偏大 | 清亮偶尔尖 | `high-pitched, rapid 7 words/s with stumbles, sometimes squealing` |
| 疲惫者 | 低（下沉） | 慢 | 低 | 气声/虚弱 | `voice dropping into a husky whisper, 2 words/s, running out of breath` |
| 新闻主播 | 中 | 中偏快 | 中 | 圆润/标准 | `centered mid-register, crisp 5 words/s, resonant and projected` |

---

## STEP 2: SIX DIMENSIONS — 六大声音维度

### 维度 1: 语速控制 (Tempo)

语速不是匀速——情绪通过**加速和减速的模式**传递。

| 语速模式 | 情绪映射 | 描述 | 标记方式 |
|---------|---------|------|---------|
| 匀速中等 | 平静/叙述 | 4-5 字/s 稳定 | `steady 4 words/s` |
| 逐渐加速 | 兴奋/焦虑升级 | 从 3→6 字/s | `accelerating from 3 to 6 words/s across the sentence` |
| 突然减速 | 强调/震惊/犹豫 | 某个词前突然从 5→2 字/s | `abrupt deceleration to 2 words/s on the key word` |
| 忽快忽慢 | 紧张/情绪失控 | 不规则节奏 | `erratic tempo swinging between 3 and 7 words/s` |
| 极慢 | 威胁/悲伤/强调 | 1-2 字/s | `each word stretched to 1s, deliberate and heavy` |
| 机关枪式 | 愤怒/争吵 | 7-8 字/s 无停顿 | `rapid-fire 8 words/s, no pauses between phrases` |

### 维度 2: 音高变化 (Pitch)

| 音高模式 | 情绪映射 | 描述 |
|---------|---------|------|
| 上行（升调） | 疑问/惊喜/不确定 | `pitch rising on the final syllable` |
| 下行（降调） | 肯定/命令/悲伤 | `pitch falling to the lowest register on the last word` |
| 平调 | 冷漠/机械/压抑 | `monotone, pitch variation less than 2 semitones` |
| 跳跃（大幅度） | 激动/夸张/讽刺 | `pitch leaping an octave on the stressed word` |
| 颤动 | 恐惧/哭泣/年老 | `pitch wavering ±1 semitone at 5Hz` |
| 挤压（气声高音） | 极度紧张/恐惧 | `voice squeezing into a tight high register, strained` |

### 维度 3: 音量动态 (Volume)

| 音量变化 | 情绪映射 | 描述 |
|---------|---------|------|
| 渐强 (crescendo) | 愤怒升级/激情 | `volume building from whisper to full projection across 3 words` |
| 渐弱 (decrescendo) | 失落/放弃/体力衰竭 | `voice fading from normal to barely audible by sentence end` |
| 突然爆发 | 爆怒/惊吓 | `volume jumping from conversational to a shout on one word` |
| 压低 | 秘密/亲密/威胁 | `dropping to a stage whisper, lips close to the listener` |
| 波动 | 哭泣中说话 | `volume surging and dropping irregularly with sobs` |

### 维度 4: 停顿设计 (Pause)

停顿是声音表演中最被低估的工具——**沉默本身就是表演**。

| 停顿类型 | 长度 | 用途 | 位置标记 |
|---------|------|------|---------|
| 呼吸停顿 | 0.3-0.5s | 自然换气 | 句子间 |
| 犹豫停顿 | 0.5-1.5s | 角色在想/选择措辞 | 句中关键词前 |
| 戏剧停顿 | 1.5-3s | 强调/震惊/情绪转折 | 重大信息后 |
| 压迫停顿 | 3-5s | 威胁/审讯/等待回答 | 提问后 |
| 崩溃停顿 | 2-4s + 非言语填充 | 情绪崩溃/失语 | 台词中断处 |

**停顿不是空白**——停顿期间应搭配非言语发声（见维度6）。

| 停顿中的填充 | 情绪 | 描述 |
|-------------|------|------|
| 吞咽声 | 紧张/压抑 | `[swallow]` |
| 深呼吸 | 控制情绪 | `[deep breath in, shaky exhale]` |
| 轻叹 | 疲惫/无奈 | `[soft sigh]` |
| 沉默+环境音 | 压迫/孤独 | `[3s silence, only clock ticking]` |

### 维度 5: 声音质感 (Voice Quality)

声音的"材质"——同一个音高和音量，不同质感传递完全不同的情绪。

| 质感 | 物理机制 | 情绪映射 | Prompt 描述 |
|------|---------|---------|------------|
| 气声 (Breathy) | 声带不完全闭合 | 亲密/脆弱/虚弱 | `breathy voice, air leaking through partially open vocal folds` |
| 颤声 (Trembling) | 声带张力不稳定 | 恐惧/悲伤/寒冷 | `voice trembling, pitch wavering at 4-6Hz` |
| 沙哑 (Hoarse) | 声带水肿/疲劳 | 久哭后/嘶吼后/疲惫 | `hoarse and raspy, as if the voice has been worn thin` |
| 紧绷 (Tight/Strained) | 喉部肌肉收紧 | 压抑愤怒/强忍痛苦 | `voice tight and constricted, words squeezed out through a clenched throat` |
| 鼻音 (Nasal) | 软腭下降 | 鼻塞/哭泣/撒娇 | `nasal quality, as if speaking through a stuffed nose` |
| 金属感 (Metallic) | 共鸣位置前移 | 冷酷/机械/疏离 | `metallic edge to the voice, hard and ringing` |
| 温暖 (Warm) | 胸腔共鸣充分 | 安慰/慈爱/温柔 | `warm chest-resonant voice, enveloping and soft` |
| 空洞 (Hollow) | 声带松弛/情绪抽离 | 创伤后/绝望/灵魂出窍 | `hollow voice, as if speaking from a great distance, no emotional color` |

### 维度 6: 非言语发声 (Non-verbal Vocalizations)

不是语言但带有意义的声音——AI 经常完全忽略这些。

详细分类见 `references/vocalization-catalog.md`。核心框架：

| 类别 | 具体声音 | 情绪映射 | Prompt 标记 |
|------|---------|---------|------------|
| 呼吸类 | 叹气、深吸气、喘息、屏息后呼出 | 各异 | `[sigh]` `[gasp]` `[sharp inhale]` |
| 喉部类 | 吞咽、清嗓、哽咽、喉咙发紧 | 紧张/压抑 | `[swallow]` `[throat clear]` `[choke]` |
| 唇部类 | 啧嘴、吹气、抿嘴吸气 | 不满/思考/疼痛 | `[tsk]` `[blowing out]` `[hissing through teeth]` |
| 笑声类 | 轻笑、苦笑、冷笑、尴尬笑、爆笑 | 各异 | `[soft chuckle]` `[bitter laugh]` `[nervous giggle]` |
| 哭声类 | 抽泣、呜咽、嚎哭、无声流泪 | 悲伤层级 | `[sob]` `[whimper]` `[wail]` `[silent tears]` |
| 犹豫类 | 嗯…、啊…、呃…、那个… | 思考/犹豫 | `[um]` `[uh]` `[er]` |
| 反应类 | 倒吸凉气、嘶声、惊叫 | 疼痛/惊恐 | `[sharp intake]` `[hiss of pain]` `[yelp]` |

---

## STEP 3: COMPOSE — 声音表演指令合成

### 台词标注格式

```
[角色名] (声音基线概要):
[非言语发声前缀] "[台词文字，{关键词用花括号标记强调}]" [非言语发声后缀]
—— 语速: [模式] | 音高: [模式] | 音量: [模式] | 质感: [类型] | 停顿: [位置+类型]
```

### 完整示例

**场景：妻子发现丈夫说谎**

❌ 没有声音指令：
```
妻子: "你昨晚到底去哪了"
丈夫: "我跟你说了 在公司加班"
```

✅ 加入声音表演指令：
```
妻子 (中高音域, 清澈质感, 正常5字/s):
[deep breath] "你昨晚…… [1.5s犹豫停顿] 到底去{哪}了。"
—— 语速: 前半句正常→"到底"处减速至2字/s | 音高: "哪"字降至最低音域 |
   音量: 全程压低于正常对话80%（冷怒比吼叫更可怕） |
   质感: 从清澈→紧绷（喉部收紧） | 停顿: "昨晚"后1.5s停顿（给丈夫压力）

丈夫 (中低音域, 正常质感, 基线4字/s):
[swallow] "我跟你说了 [0.3s] 在—— [0.5s犹豫] 在公司加班。"
—— 语速: "我跟你说了"加速至6字/s（急于辩解）→"在"字重复（说谎犹豫） |
   音高: 整体比基线上移2个半音（紧张性升调） |
   音量: 正常 | 质感: 微弱气声混入（说谎时声带闭合度下降） |
   停顿: "在"字前0.5s犹豫停顿 + 吞咽声（经典说谎信号）
```

**场景：战友牺牲后的独白**

```
士兵 (低沉音域, 沙哑质感, 基线3字/s):
[3s silence, only wind sound] [shaky exhale]
"他…… [1s] 他说…… [2s崩溃停顿, 嘴巴张开又闭上] ……
他说{明天}……回家。" [voice breaks into choked sob]
—— 语速: 极慢1-2字/s，每个词之间都有间隙 |
   音高: "明天"突然上扬（回忆战友的声音）→"回家"坠至最低（现实打击） |
   音量: 全程极低，仅够听清 | 质感: 沙哑→颤声→声音断裂（cracking） |
   停顿: 多处崩溃停顿，用非言语发声填充（呼吸/吞咽/嘴巴动作）
```

---

## STEP 4: PLATFORM ADAPTATION — 平台适配

不同平台对声音指令的接收方式不同：

### TTS 工具（speech agent）

| 维度 | 可通过参数控制 | 需要在文本中标注 |
|------|-------------|----------------|
| 语速 | `speed` 参数 | 句内变速需文本标注 |
| 音高 | 部分支持 | 标注 `[升调]` `[降调]` |
| 停顿 | 标点+SSML 标记 | `[pause 1.5s]` 或 `……` |
| 质感 | `emotion` 参数（粗粒度） | 文本描述辅助 |
| 非言语 | 不直接支持 | 单独生成后拼接 |

### Seedance 视频内置语音

| 维度 | 控制方式 |
|------|---------|
| 语速/音调/质感 | 在 prompt 中用自然语言描述 |
| 停顿 | 台词中用 `...` 或描述 `pauses for 2 seconds` |
| 非言语 | 直接写入 prompt（`the character sighs before speaking`） |

### 生成后编辑（editing agent）

对生成的音频进行后处理：
- 手动插入停顿（静音段）
- 叠加环境音填充停顿
- 调整音量动态（压缩/扩展）

---

## STEP 5: SELF-CHECK — 声音表演检查

- [ ] 每段台词都标注了至少 2 个声音维度（不是全部平铺）
- [ ] 声音质感和角色的当前情绪一致
- [ ] 停顿位置有戏剧功能（不是随机的沉默）
- [ ] 非言语发声合理存在（紧张→吞咽，悲伤→叹气，不要张冠李戴）
- [ ] 语速变化有方向（渐快/渐慢/突变），不是匀速
- [ ] 如果有多角色对话，每个角色的声音基线可区分
- [ ] 声音指令和视觉指令（表情/动作）的情绪方向一致

---

## STEP 6: PRESENT & ITERATE

输出完整的声音表演标注脚本。

如果用户反馈"还是太平"：
1. 增加音量对比（某个词突然变大或变小）
2. 在关键台词前增加 2-3s 的戏剧停顿
3. 给角色加一个标志性的声音习惯（如反派总是在威胁前轻笑一声）
4. 加载 `references/vocalization-catalog.md` 获取更丰富的非言语发声库
