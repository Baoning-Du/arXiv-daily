# arXiv hep-th Daily Digest (Cloud)

GitHub Actions 自动汇总 arXiv hep-th（高能理论物理）每日新论文。
每天早上 9:00（北京时间）自动运行，抓取前一天论文，AI 翻译中文并生成评价，
推送到你的个人微信（通过 PushPlus）。

## 快速开始

### 1. Fork 或创建仓库

```bash
git init
git remote add origin https://github.com/YOUR_USERNAME/arxiv-hep-th-daily.git
```

### 2. 获取 DeepSeek API Key

1. 访问 [platform.deepseek.com](https://platform.deepseek.com)
2. 注册并充值（最低充 ¥1 即可，每天约 ¥0.01-0.05）
3. 在「API Keys」页面创建一个 Key

### 3. 获取 PushPlus Token（推送到微信）

1. 微信扫码关注 **PushPlus** 公众号（访问 [pushplus.plus](https://www.pushplus.plus) 扫码）
2. 在公众号菜单点「个人中心」，复制你的 **Token**

### 4. 配置 GitHub Secrets

在 GitHub 仓库页面 → **Settings** → **Secrets and variables** → **Actions** → **New repository secret**，添加两个 Secret：

| Secret 名称 | 值 |
|------------|-----|
| `DEEPSEEK_API_KEY` | 你的 DeepSeek API Key |
| `PUSHPLUS_TOKEN` | 你的 PushPlus Token |

### 5. 推送到 GitHub

```bash
git add .
git commit -m "arxiv hep-th daily digest setup"
git push origin main
```

### 6. 测试

在 GitHub 仓库页面 → **Actions** → 点击左侧「arXiv hep-th Daily Digest」→ **Run workflow** → 手动触发一次测试。

## 工作流程

```
每天 09:00 (北京时间)
  │
  ├─ 计算昨天日期
  ├─ 从 arXiv API 抓取 hep-th 论文
  ├─ 调用 DeepSeek API 逐篇翻译 + 评价
  ├─ 生成 HTML 报告
  ├─ 清理中间文件
  ├─ 推送通知到个人微信（PushPlus）
  └─ 上传 HTML 到 GitHub Artifacts（保留 90 天）
```

## 费用

- **GitHub Actions**：免费（公开仓库无限，私有仓库每月 2000 分钟）
- **DeepSeek API**：约 ¥0.01-0.03/天（约 10 篇论文），¥0.3-1/月
- **总计**：≈ ¥1/月

## 文件结构

```
.
├── .github/workflows/daily.yml    # GitHub Actions 工作流
├── scripts/
│   ├── run.py                     # 主脚本：一站式抓取→翻译→生成HTML
│   ├── fetch_papers.py            # 抓取 arXiv API（分模块）
│   ├── translate_cloud.py         # DeepSeek API 翻译（分模块）
│   ├── build_html.py              # 生成 HTML 报告（分模块）
│   └── build_hub.py               # 生成 Hub 首页（分模块）
├── outputs/                       # HTML 输出目录
├── .gitignore
└── README.md
```
