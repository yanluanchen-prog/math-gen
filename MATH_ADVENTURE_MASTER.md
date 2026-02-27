# Math Adventure — 母版（Golden Master）

母版文件：`math-adventure-master.html`
SHA256：`a31710837931efec6c32a1c7217185fb655f78587f46c2fdae8a565cc754fbbe`

## 使用原则（避免再出现“改一处坏一片”）
1) **永远从母版复制一份再改**：不要在历史旧版本上打补丁。
2) 每次改动必须先做 **3 项自检**：
   - `node --check`（脚本语法）
   - 本地 Chrome 打开：Home → Map → 开始闯关 → 方法提示 → 家长面板（主链路不报错）
   - iPad/Safari（或移动端）点一次屏幕后：音乐/语音/音效能否触发（受自动播放策略影响）
3) 每次交付都加一个**版本号**（例如页面标题旁小字），方便确认不是缓存。
4) 地图居中规则：进入地图默认火星居中；解锁推进后始终保持“最新可进入关卡/星球”居中。

## 核心模块清单（改动优先看这里）
- 地图渲染：`renderMap()`
- 关卡启动：`startLevel()` / `loadQ()`
- 倒计时：`TIME_LIMIT` / `startTimer()` / `tick()`
- 方法提示：`showHint()` / `closeHint()`（含暂停计时与 sawHintThisQ 逻辑）
- 家长入口：`askParentGate()` / `openParent()`
- 音效：`sfxTone()` / `sfxCorrect()` / `sfxWrong()` / `sfxVictory()`
- 数据存储：`loadData()` / `save()`（localStorage）

## 标准交付流程（你让我“继续优化”时我会按这个走）
1) 从 `math-adventure-master.html` 复制成 `math-adventure-work.html`
2) 在 work 文件上修改
3) 自检通过后：
   - 发回 `math-adventure-work.html`
   - 如需要上线：替换 GitHub Pages 的 `index.html`
4) 若该版本稳定：再把 work 覆盖回母版并更新 SHA256。
