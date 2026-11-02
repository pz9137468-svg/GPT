# 个性化 GPT 本地记忆摘要｜2026-W44

覆盖周期：2026-10-26 至 2026-11-01

本文件是从本地 Codex 记忆摘要中提取的脱敏版本，只保留可复用偏好、知识和改进点；不包含原始对话、密钥或本机绝对路径。

## 稳定偏好

- When the user reported that selecting “图片、视频、音频等” did not make folder changes apply together and asked to inspect “等等类似的问题,” future fixes should audit all related batch metadata/file operations under the same selection model, not only the named dropdown.
- Name, source URL, and prompt were intentionally kept single-item-only; do not batch-overwrite unique per-file fields without an explicit contract.
- 用户说“加入个困难模式”并要求保留原有体验 -> 类似改动应默认保留普通模式、存档和路线。
- 用户要求“优化怪物和角色的动作关键帧动画，有大幅度变化” -> 需要提供明显可感知的动作变化，并同步命中判定，而非只改数值。
- 用户要求“搜索全网顶级的场景概念设计图，尤其是 A 站上面的”，并要求覆盖电影、游戏及构图、纵深、配色、透视、明度、饱和度、疏密、氛围、空气透视、尺度和反差 -> 类似任务应按来源类别组织，并将案例分析转化为可复用规则。
- 用户要求“生成一份研究报告”并“生成10套适用于MJ的英文提示词” -> 交付应包含完整方法论和可复制产物，不只是案例列表。
- 用户希望“举一反三”而非复制具体电影或游戏画面 -> prompt 应使用原创世界观和通用视觉机制，避免直接复刻特许经营或在世艺术家个人风格。
- 既有用户工作流强调 MJ 草案未实际渲染前必须标记为未验证；本报告也明确提示词尚未生成验证。
- 用户明确说“不要参考我这个红楼仙踪” -> 类似任务应排除用户项目中的峰系、法器、世界观和专属职位，只使用原著人物锚点。
- 用户要求“属于什么类型角色，适配什么类型服饰，概括总结上述每个角色” -> 输出应为逐角色结构化总结，至少包含角色类型、服饰方向和性格概括。
- 最终采用 N（原著锚点）/ I（创作推演）区分 -> 应明确标注哪些是原著依据、哪些是新创作适配，避免把推演内容当作原著设定。
- When the user said “生图时，对话里没提到什么模型，默认GPTimage-2，最高细节精度，4k画质” -> default future image requests to `gpt-image-2`, `--quality high`, and 4K while honoring explicit model/size/format/quality choices.
- The user explicitly confirmed that the old key file should be primary and the newer key file backup -> determine credential roles from the user’s stated priority, not filename naming.
- The user said: "按照这个质量，再生成9个完全不一样的" -> future batches should vary weapon silhouettes and structures genuinely, not just recolor existing designs, while preserving the established premium rendering quality.

## 可复用知识

- `src/app.js` uses selected-target resolution for inspector batch edits; `electron/main.cjs` provides `updateAssetsBatch()` and transactional `moveAssetsBatch()`.
- `moveAssetsBatch()` deduplicates/caps selections at 500, validates all external watched-folder paths before mutation, snapshots state, and rolls back filesystem failures.
- Native external drag/copy uses real absolute paths. `preparedNativeDragPaths()` stages prefixed or colliding names as hard links under `.youyou/native-drag`; it falls back to validated originals on hard-link failure and never transcodes or compresses media.
- macOS supports full multi-file `NSFilenamesPboardType`; Windows Electron fallback cannot reliably guarantee multi-file `CF_HDROP`, so warn and guarantee only the first local file.
- Root cause was viewport-coordinate endpoints plus scanning only mounted `cardById` cards; virtual scrolling unloaded the starting cards and dropped them from selection.
- The fix stores marquee endpoints in grid content coordinates, projects the last pointer position after scroll, computes intersections from `virtualGrid.assets` geometry, scans only covered rows, and cancels safely on layout changes.
- Key functions: `marqueeContentPoint`, `marqueeIntersectedAssetIds`, `activateMarqueeFromCurrent`, `handleAssetScroll`, `cancelMarqueeForLayoutChange`.
- `scripts/test-marquee-scroll.cjs` covers down/up scrolling, pullback, off-screen cards, scroll-only activation, additive Cmd/Ctrl selection, list view, and 10k-item bounded scanning.
- `npm run check` passed with `marquee scroll tests ok: down, up, pullback, scroll activation, virtual rows, additive, list view, 10k bound`.
- Artifacts: `release/YouYou-Library-macOS-v0.8.32.zip` and `release/YouYou-Library-Windows-v0.8.32.zip`.
- Mac/Windows/installed ASAR SHA-256 matched `1ba0ffff3853dd339040ecd1d1c91b7b687c9168c0ea3163cce8fc83d8c72e94`.
- `unzip -t` passed for both archives; macOS `codesign --verify --deep --strict` passed; `/Applications/友友库.app` reported version `0.8.32` and launched successfully.
- Old v0.8.31 artifacts were moved to Trash rather than force-deleted; release directory retained only v0.8.32 outputs.
- 困难模式参数集中在 `scripts/game.gd`：普通敌人伤害 +1、敌人前摇缩短 10%、收招间隔缩短 20%、香结上限 2、每次治疗 2；不提高敌人血量、不改变跳跃或阴阳规则。

## 需要避免或复盘

- The original bug was renderer-only: backend batch APIs already existed, but inspector handlers used `state.inspectedId` alone. Inspect renderer target resolution, preload IPC, main-process validation, and persistence together.
- Packaging first failed with `connect ETIMEDOUT 20.205.243.166:443`; use the cached Electron archive with `--electron-zip-dir` when network download is unavailable.
- `src/app.js`: `inspectorEditAssets`, `persistInspectorUpdates`, `moveAssetsToCollection`.
- `electron/main.cjs`: `updateAssetsBatch`, `moveAssetsBatch`, `preparedNativeDragPaths`, `copyAssetsToClipboard`.
- `node scripts/test-native-media-drag.cjs` passed mixed-media drag staging, IPC, clipboard, rollback, move, and 500-item-cap tests.
- 曾出现测试脚本数组索引错误；之后通过单独构造敌人测试对象修复。未来应让测试辅助对象与实际 room enemy 数组解耦。
- 用户指出芦苇怪原画朝向翻反、前肢拉伸过度、误用扫桨特效；后续类似动作改动必须做实机慢放/朝向检查，不应只验证 AnimationPlayer 轨道存在。
- `scripts/game.gd`, `scripts/enemy.gd`, `scripts/player.gd`, `scripts/world.gd`, `scripts/interface.gd`, `scripts/motion_library.gd`
- `tests/runner.gd`, `qa/test_results.json`
- Do not claim Windows runtime success from PE inspection or Mac Godot `--main-pack` loading. The rollout correctly reported “未在 Windows 实机运行验证、未做数字签名”.
- `制作资料/Windows导出_V0.4.2/verify_and_package.py`
- `制作资料/Windows导出_V0.4.2/verification.json`
- EXE SHA-256: `ef4f08eaba55b84cc247efbea9035298309ce9c6e7e8dbb234a4874fe1836a79`
- ZIP SHA-256: `c7b0a152f6618a9daea083407606d2c12e43cc3b34b732d4e57451446a9d7f99`

## 使用方式

下一周生成时会重新读取本地记忆源，并将新的条目合并到对应周目录；GitHub 只同步本摘要文件。
