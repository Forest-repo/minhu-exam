# 民呼我为·机考系统

党建引领"民呼我为"工作接诉专项个人赛笔试配套的内部刷题/模考系统。

**在线使用**：https://forest-repo.github.io/minhu-exam/ （内部系统，需访问口令）

## 功能

- **模拟考试**：14 套试卷（正卷 1 + 备用卷 1 + 模拟卷 12），每套 75 题 100 分，60 分钟倒计时，交卷立即出分
- **严格判分**：多选 / 填空全对才得分，填空逐字比对（含标点）
- **错题本**：答错自动收录，记录错误次数与历史答案；同一题连续答对 2 次自动移出
- **错题重练**：一键把错题本组成新卷再考
- **背题模式**：按套浏览题目与答案，支持打印
- **数据管理**：成绩与错题记录保存在浏览器 localStorage，支持 JSON 导出 / 导入换机

## 技术说明

- 单文件应用：`index.html` 内嵌全部题目数据与逻辑，零依赖、零后端
- 题库来源：`bank.json`（551 题，解析自官方题库 docx，含 5 处瑕疵自动修复）
- 出卷流水线：`parse_bank.py` → `sample_sets.py` → `render_docs.py` / `build_exam_app.py` → `verify_all.py`
- 判分逻辑经 node 单测覆盖，页面经 Playwright 端到端实测

## 更新部署

```bash
# 1. 修改生成脚本后重建
python build_exam_app.py
# 2. 拷贝为站点入口
cp 输出/机考系统.html site/index.html
# 3. 通过 GitHub REST API 更新（本网络下 git push 走代理易断，用 gh api 更稳）
gh api repos/Forest-repo/minhu-exam/contents/index.html -X PUT \
  --input body.json   # body.json 含 message + base64(content) + sha
```

## 本地运行

直接双击 `index.html`，或：

```bash
python -m http.server 8000
# 打开 http://localhost:8000
```
