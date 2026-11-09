# 个性化 GPT 本地记忆摘要｜2026-W45

覆盖周期：2026-11-02 至 2026-11-08

本文件是从本地 Codex 记忆摘要中提取的脱敏版本，只保留可复用偏好、知识和改进点；不包含原始对话、密钥或本机绝对路径。

## 稳定偏好

- When the user said image generation without a named model should use GPT Image 2, highest detail, and 4K -> default future requests to `gpt-image-2`, `quality=high`, and 4K dimensions unless explicitly overridden.
- The user confirmed: old key file should be primary and newer key file should be backup -> follow the explicit role mapping, not filename semantics.
- 用户明确要求“要有学术规范有明确的引用源” -> 默认提供原始发布者、标题、日期、来源页、访问日期、图片类型、图注和参考文献格式，不要只给图片搜索结果。
- 用户需要角色设计图片 -> 将角色设定图、概念艺术、宣传图、实机截图分别标注，避免混淆证据类型。
- 用户任务聚焦于“赛博朋克、女性机械改造、呼吸面罩、义体、霓虹色彩、游戏角色三维设计”相关研究，类似任务应同时提供贴图特征、理论关键词和制作流程关键词，而不是只给宽泛风格词。
- 该任务要求区分“知网可检索”与“已经打开并核对知网详情页”；未来检索应明确标注每条题录的核验等级。
- 用户反复要求“页数是啥”“你有办法看到相关论文不”，说明未来应提供来源层级和核查边界，不能仅凭题名、摘要或目录声称读过全文。
- 用户需要区分论文总页数和脚注引文页码；总页数不能代替具体引用页码。
- 用户希望在CNKI受限时获得“其他网站可以查询”的实际路径；未来应区分检索可见性与全文下载权限。
- 用户截图中的结果是作者相关期刊文章，不是目标学位论文；核对文献类型、完整题名和DOI后再引用。
- 用户要求参考《边缘行者》但“完全不一样” -> 类似任务应借鉴建立关心、代价升级、核心转折和伏笔回收等方法，不能复用原作人物、事件、关系或结局。
- 用户接受并使用完整系列策划格式：世界观、人物、8集分集、转折、伏笔、影像方向和后续方向，而非只有一句梗概。
- 用户要求两张图作为同一角色的外形参考，但设计三套不同身份、性格和剧情 -> 保持共享视觉锚点，同时显著区分职业、内在冲突、玩法和结局。
- 类似任务应主动包含玩家关系、能力边界、任务循环、两次转折和分支结局，而不止写外貌或性格标签。

## 可复用知识

- `<local-path>` now defaults to `gpt-image-2`, `3840x2160`, `high`, and PNG; portrait 4K is `2160x3840`.
- Actual dimensions must be verified; never label a smaller or upscaled output as native 4K.
- The config describes a chat/coding provider, not image generation: `model` selects the model, `model_provider` selects the provider block, `base_url` identifies the internal service, `wire_api="responses"` selects the protocol, and `requires_openai_auth=false` avoids OpenAI login credentials.
- Current mapping is primary `<local-path>`, backup `<local-path>`.
- GPT fallback remains bounded to one retry with identical parameters; `--profile backup` selects backup and `--no-backup` disables fallback.
- Fallback excludes Gemini, local I/O, and content-policy failures.
- Offline verification: `PASS: old key primary, newer key backup, one automatic fallback; GPT/high/4K defaults preserved. No API requests sent.`
- `/models` returned HTTP 200 and listed `gpt-image-2`.
- `/v1/dashboard/billing/credit_grants` returned HTTP 404; the gateway exposed no usable balance endpoint.
- A primary-only JPEG generation succeeded with no fallback: valid `3840x2160` JPEG, about 72 seconds. This proves current generation access, not remaining balance.
- 官方赛博朋克新闻单页 API 可用：`<url>`；返回 `news_title`、`news_date`、`news_full` 等字段，`news_full` 中的 `<img src>` 可提取原始图片 URL。
- 已核验的官方来源包括：`/en/news/21044/concept-art-from-the-trailer`（2018-06-12）、`/en/news/22427/concept-art`（2018-08-22）、`/en/news/28441/c-usb-01-backup-concept-art-cp-visual-styles`（2019-06-17）、`/en/news/48760/lizzy-wizzy-will-appear-in-phantom-liberty-wearing-a-special-headpiece-designed-by-japanese-artist-ikeuchi`（2023-09-02）、以及 `<url>`。
- Cosplay Guides 页面提供 V、Royce、Placide、Brigitte、Jackie、Meredith Stout、Dexter DeShawn、Trauma Team 等角色的服装、义体、配件和局部细节。高清包示例：`<url>`。
- 本地资料包路径：`<local-path>`；包含 `index.html`、`素材链接总索引.csv`、`README_扩展素材来源.md`、`参考文献与图注.md`、样图和验证记录。

## 需要避免或复盘

- No live image was generated during installation; installation success was verified through `status`, help output, file comparison, and later live testing.
- `<local-path>`
- Status fields: `default_size: 3840x2160`, `default_quality: high`, `default_output_format: png`.
- Official config documentation returned HTTP 403; treat provider behavior not directly established by the fields as uncertain.
- Never retain or repeat the bearer token supplied in the TOML; it was redacted from memory.
- Error: `HTTP 403` from `<url>`.
- First primary-only 4K PNG request returned truncated JSON after about 90 seconds: `Unterminated string starting at: line 6 column 19 (char 89)`, with no saved image. This is a transport/response-integrity failure, not proof of quota exhaustion.
- A smaller JPEG response succeeded; preserve the output and verify with `file` and `sips` before reporting dimensions.
- Test directory: `<local-path>`
- Verified output: `primary-quota-test.jpg`
- Verification: `JPEG image data ... 3840x2160`; `pixelWidth: 3840`, `pixelHeight: 2160`.
- Google、ArtStation 等访问不稳定；未来优先使用官方 API、文章页面和公开文件字段，不要把搜索摘要或不可访问页面当作已验证来源。
- Pillow 缺失时使用 `file`、图片头信息和项目研究工具；下载后最好同时验证 SHA-256、格式、尺寸和 `Image.verify()`。
- 对带 `@`、空格或特殊字符的 URL 使用逐条引用的完整 URL，避免 shell 分词导致下载失败。

## 使用方式

下一周生成时会重新读取本地记忆源，并将新的条目合并到对应周目录；GitHub 只同步本摘要文件。
