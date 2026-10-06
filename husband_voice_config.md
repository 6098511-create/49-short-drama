# 丈夫角色正式声音配置

建立日期：2026-10-03（Asia/Shanghai）。

人工确认及锁定日期：2026-10-03（Asia/Shanghai）。

## 角色与声音要求

- 角色 ID：husband。
- 角色：丈夫，51岁，男性。
- 年龄感：约51岁。
- 语言：普通普通话，日常说话方式。
- 表演方向：成熟、平静、克制，有距离感。
- 避免夸张表演、影视腔和播音腔。

## 当前状态

- 状态：locked（已人工试听确认，正式声音标准已锁定）。
- selected_candidate：H02。
- 正式声音：H02｜F5-TTS｜成熟有距离感。
- 确认依据：用户在本轮明确表示已人工试听确认，并要求锁定 H02。
- 适用角色：丈夫。仅锁定声音标准；本次不启动任何剧集的正式配音。
- 后续丈夫配音沿用下列模型、参考音、参数及输出处理；每个生成单元显式使用 seed=2202，不随机换 seed，不自动改选音色。
- 本次仅更新本文件，不修改 EP01 视频或其他角色配置，不生成音频。

## H02 正式模型与生成配置

### 模型与环境

| 项目 | 锁定值 |
| --- | --- |
| 模型 | F5TTS_v1_Base |
| 软件版本 | f5-tts 1.1.22（当前已核查环境） |
| 权重 | `E:\wok\2026-09-30-13-45-08\f5-models\F5TTS_v1_Base\model_1250000.safetensors` |
| 词表 | `E:\wok\2026-09-30-13-45-08\f5-models\F5TTS_v1_Base\vocab.txt` |
| 声码器 | Vocos 24kHz |
| 声码器目录 | `E:\wok\2026-09-30-13-45-08\f5-models\vocos-mel-24khz` |
| Python | `C:\Users\Administrator\.workbuddy\binaries\python\envs\ttsgpu\Scripts\python.exe` |
| device | cuda |
| use_ema | true |
| ode_method | euler |

### 固定推理参数

| 参数 | 锁定值 |
| --- | --- |
| cfg / cfg_strength | 3.2 |
| speed | 0.90 |
| seed | 2202 |
| nfe / nfe_step | 48 |
| sway_sampling_coef | -1 |
| target_rms（推理） | 0.1 |
| cross_fade_duration | 0.15 秒 |
| fix_duration | null |
| remove_silence | false |
| file_wave / file_spec（推理调用） | null / null；返回音频后按下述规则处理并保存 |

配置依据：`E:\wok\2026-09-30-13-45-08\gen_husband_cast5.py` 中 V2 任务及显式参数。ode_method、cross_fade_duration、fix_duration、file_spec 在旧脚本中未显式传入，本轮只读核对本地 `f5_tts/api.py` 默认值后明确登记，后续须显式保持一致。历史试听没有独立环境快照，不将当前源码核查表述为历史环境的完整证明。

### 固定参考音与试听基准

- reference：`ref_V2.wav`。
- 正式参考音路径：`E:\wok\2026-09-30-13-45-08\f5-refs-husband5\ref_V2.wav`。
- 参考音 SHA-256：`235bdd0ce5cf198342dce87558e4745a1bff6b39a94f2c1f86ece20c1d0d6c79`（本轮已复核）。
- 固定参考文本：`我们分开吧。我们分开吧。我们分开吧。`
- 已选试听文件：`D:\文档\ChatGPT\《49岁，我决定重新活一次》短剧\49岁我决定重新活一次\EP01\05_配音\06_丈夫最终音色5版\丈夫_版本2_成熟有距离感.wav`。
- 试听 SHA-256：`094aaf85a8cbdd54fe2e7949c70ff46efafad90c3cff62f6550c060a8d6fdbb3`（本轮已复核）。
- 试听生成文本：`我们分开吧。`；试听时长：0.943667秒。
- 试听文件作为听感基准，推理参考输入使用上述丈夫专用 ref_V2.wav；不能误用林晚同名参考音，也不能用试听输出替代参考输入。

### 输出处理

- 沿用 H02 生成脚本：首尾裁切阈值0.008；句首最多保留0.10秒，句尾最多保留0.25秒。
- 裁切后按实际峰值计算线性增益，将峰值归一至0.89。target_rms=0.1 是推理参数，后处理不额外采用林晚 A3 的 RMS 目标增益规则。
- 保留内部停顿，不额外变调或改变语速。
- 输出：24kHz、单声道、PCM16 WAV。
- 后续正式配音另行执行并验收；声音标准锁定不自动确认已有整集音频。

## 历史候选记录（正式采用 H02）

历史生成脚本记录的统一试听文本：**我们分开吧。** 用户已人工试听确认 H02；其他候选仅保留作历史记录，不属于正式声音配置。

| 候选 | 试听方案 | 模型 / 声音 | 参数 | 时长 |
| --- | --- | --- | --- | --- |
| H01 | F5 自然日常对话 | F5TTS_v1_Base | cfg=2.4；speed=0.95；seed=1101；nfe=48；reference=ref_V1.wav | 0.857250 秒 |
| H02 | F5 成熟有距离感 | F5TTS_v1_Base | cfg=3.2；speed=0.90；seed=2202；nfe=48；reference=ref_V2.wav | 0.943667 秒 |
| H03 | F5 极度克制认命 | F5TTS_v1_Base | cfg=2.8；speed=0.86；seed=3303；nfe=48；reference=ref_V3.wav | 0.970417 秒 |
| H04 | Kokoro 云健 | Kokoro ONNX（kokoro-v1.0.onnx） | voice=zm_yunjian；speed=0.95；lang=cmn | 1.472000 秒 |
| H05 | Edge 云希 | Edge TTS（云端 Neural voice） | voice=zh-CN-YunxiNeural；rate=-5%；pitch=-15Hz | 2.040000 秒 |

### 试听文件

- H01：[F5 自然日常对话](D:/文档/ChatGPT/《49岁，我决定重新活一次》短剧/49岁我决定重新活一次/EP01/05_配音/06_丈夫最终音色5版/丈夫_版本1_自然日常对话.wav)
- H02：[F5 成熟有距离感](D:/文档/ChatGPT/《49岁，我决定重新活一次》短剧/49岁我决定重新活一次/EP01/05_配音/06_丈夫最终音色5版/丈夫_版本2_成熟有距离感.wav)
- H03：[F5 极度克制认命](D:/文档/ChatGPT/《49岁，我决定重新活一次》短剧/49岁我决定重新活一次/EP01/05_配音/06_丈夫最终音色5版/丈夫_版本3_极度克制认命.wav)
- H04：[Kokoro 云健](D:/文档/ChatGPT/《49岁，我决定重新活一次》短剧/49岁我决定重新活一次/EP01/05_配音/03_丈夫选角试听/丈夫选角_zm_yunjian_speed0.95.wav)
- H05：[Edge 云希](D:/文档/ChatGPT/《49岁，我决定重新活一次》短剧/49岁我决定重新活一次/EP01/05_配音/03_丈夫选角试听/丈夫选角_edge_Yunxi_pitch-15.wav)

## 模型与参数来源

### H01–H03：F5-TTS

- 参数来源：`E:\wok\2026-09-30-13-45-08\gen_husband_cast5.py`；读取脚本，未运行。
- 模型：F5TTS_v1_Base；当前 ttsgpu 环境安装版本：f5-tts 1.1.22。历史样本未附独立环境快照，不把当前版本核查当作其历史版本证明。
- 权重：`E:\wok\2026-09-30-13-45-08\f5-models\F5TTS_v1_Base\model_1250000.safetensors`。
- 词表：`E:\wok\2026-09-30-13-45-08\f5-models\F5TTS_v1_Base\vocab.txt`。
- 声码器：Vocos，目录 `E:\wok\2026-09-30-13-45-08\f5-models\vocos-mel-24khz`。
- Python：`C:\Users\Administrator\.workbuddy\binaries\python\envs\ttsgpu\Scripts\python.exe`。
- 共用显式设置：device=cuda、use_ema=true、sway_sampling_coef=-1、target_rms=0.1、remove_silence=false、nfe_step=48。各候选 cfg_strength、speed、seed 见表。
- 参考文本：`我们分开吧。我们分开吧。我们分开吧。`
- 历史输出处理：首尾裁切阈值0.008，句首最多保留0.10秒、句尾最多保留0.25秒，峰值归一至0.89，写为PCM16 WAV。
- 历史脚本未显式写入的引擎默认参数没有批次快照；H02 当前锁定值及核查依据见前文正式配置。

| 候选 | 参考音路径 | SHA-256 |
| --- | --- | --- |
| H01 | `E:\wok\2026-09-30-13-45-08\f5-refs-husband5\ref_V1.wav` | `66ed4c819589cfa1f39c68d72bf15edd9c1883943d44f5dade8fdcd853ace1fa` |
| H02 | `E:\wok\2026-09-30-13-45-08\f5-refs-husband5\ref_V2.wav` | `235bdd0ce5cf198342dce87558e4745a1bff6b39a94f2c1f86ece20c1d0d6c79` |
| H03 | `E:\wok\2026-09-30-13-45-08\f5-refs-husband5\ref_V3.wav` | `4d8cad0d297c72e3b8d35263cbdf9fa59f6979c2ff915d63cbb97c4ffcf7a25c` |

丈夫 H02 的 ref_V2.wav 位于 f5-refs-husband5；林晚同名参考音位于 f5-refs-v3，二者不可混用。

### H04：Kokoro 中文 TTS

- 参数来源：`E:\wok\2026-09-30-13-45-08\gen_cast_round2.py`；读取脚本，未运行。
- 模型文件：`E:\wok\2026-09-30-13-45-08\tts-models\kokoro-v1.0.onnx`。
- 音色库：`E:\wok\2026-09-30-13-45-08\tts-models\voices-v1.0.bin`。
- voice=zm_yunjian，speed=0.95，lang=cmn；脚本未设置 seed，不使用克隆参考音。

### H05：Edge TTS

- 参数来源：同上 `gen_cast_round2.py`。
- voice=zh-CN-YunxiNeural，rate=-5%，pitch=-15Hz；无本地模型权重，云端模型具体版本未记录。
- 历史脚本通过 edge_tts.Communicate 合成，再用 FFmpeg 转换为24kHz、单声道、PCM16 WAV。
- 当前仅验证已有样本，未调用云端服务，未验证现时在线生成可用性。

## 候选建立阶段的只读测试记录

- 五份试听均可完整读取PCM样本，格式均为24kHz、单声道、16位；均非全静音，未检测到达到PCM正负极限的样本。
- F5 权重、词表、Vocos配置与权重、三份参考音均存在；Kokoro模型与音色库均存在。
- 指定 ttsgpu 环境含 f5-tts 1.1.22；该环境未安装 edge-tts、kokoro-onnx，其他环境未核查。历史样本可供试听不代表三条链路均已通过当前推理测试。
- 本轮不安装依赖，不生成或转换音频，不对原试听做响度统一。不同样本响度存在差异。
- 候选建立阶段仅做文件技术检查。此后用户已人工试听确认 H02 为“成熟有距离感”的丈夫声音；其他候选不自动标记为通过听审。

| 候选 | 峰值（线性） | RMS | PCM削波样本数 | 音频 SHA-256 |
| --- | --- | --- | --- | --- |
| H01 | 0.890015 | 0.073717 | 0 | `92086fe1f1abd077fb9517b3ec7061754d18a92bc5ff0e37789465b8bd7888fa` |
| H02 | 0.890015 | 0.069098 | 0 | `094aaf85a8cbdd54fe2e7949c70ff46efafad90c3cff62f6550c060a8d6fdbb3` |
| H03 | 0.890015 | 0.078107 | 0 | `5f3b33fb446cf6c5a20fd77a29a074d71c1ad295e89ab566fa3b2c3507daafa5` |
| H04 | 0.591919 | 0.100433 | 0 | `3d134e70bcbb2bc8fe98050e67ba1fe7211eba69bf78ff319a07a9a21657007d` |
| H05 | 0.355621 | 0.033345 | 0 | `88ded96b9d80ac04099442181498fa85a472dfb137a45bb4cc26a7ca60545875` |

## 人工选择记录

- 选择结果：H02｜F5-TTS｜成熟有距离感。
- 确认日期：2026-10-03（Asia/Shanghai）。
- 听审确认：用户明确表示已人工试听确认并选择 H02；声音要求按本文件“角色与声音要求”锁定。
- 正式适用范围：丈夫角色；不修改其他角色声音标准。
- 本次完成范围：仅声音配置锁定。未运行 TTS，未生成正式配音，未修改 EP01 视频；完成后停止。
