# GitHub 统计和贪吃蛇设置指南

## 📊 GitHub 统计卡片（已配置，无需额外设置）

当前 README 中已包含统计卡片，直接可用：
- 统计卡片：显示你的 GitHub 统计数据
- 语言统计：显示你最常用的编程语言

**只需将 `HarmmerRay` 替换为你的 GitHub 用户名即可。**

---

## 🐍 贪吃蛇贡献图设置步骤

### 步骤 1：创建同名仓库

1. 在 GitHub 上创建一个新仓库
2. **仓库名必须与你的 GitHub 用户名完全相同**（例如：如果你的用户名是 `yourusername`，仓库名也必须是 `yourusername`）
3. 设置为 **Public（公开）**
4. 初始化 README（可选）

### 步骤 2：创建 GitHub Actions 工作流

1. 在仓库中创建 `.github/workflows/` 目录
2. 创建文件 `.github/workflows/snake.yml`
3. 添加以下内容：

```yaml
name: Generate Snake Animation

on:
  schedule:
    # 每天 UTC 时间 0:00 运行（北京时间 8:00）
    - cron: '0 0 * * *'
  workflow_dispatch:  # 允许手动触发

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Generate snake animation
        uses: Platane/snk@master
        id: snake-gif
        with:
          github_user_name: ${{ github.repository_owner }}
          outputs: |
            dist/github-contribution-grid-snake.svg

      - name: Deploy to output branch
        uses: peaceiris/actions-gh-pages@v3
        if: always()
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
          destination_dir: output
```

### 步骤 3：更新 README 引用

在你的同名仓库的 `README.md` 中添加：

```markdown
## 🐍 Contribution Snake | 贪吃蛇贡献图

![Snake animation](https://raw.githubusercontent.com/你的用户名/你的用户名/output/github-contribution-grid-snake.svg)
```

### 步骤 4：等待首次运行

- GitHub Actions 会在每天自动运行一次
- 你也可以在 Actions 标签页手动触发
- 首次运行后，会在仓库中创建 `output` 分支，并生成 SVG 文件

### 步骤 5：在你的主 README 中引用

在你的个人主页 README（`11.md` 或其他 README）中，使用以下格式：

```markdown
<img src="https://raw.githubusercontent.com/你的用户名/你的用户名/output/github-contribution-grid-snake.svg"/>
```

---

## 🔧 常见问题

### Q: 统计卡片不显示？
A: 
1. **检查用户名**：确保 `username=HarmmerRay` 中的用户名正确
2. **检查仓库可见性**：确保至少有一些公开仓库
3. **清除缓存**：在 URL 后添加 `&cache_seconds=0` 强制刷新
4. **等待加载**：GitHub 图片可能需要几秒钟加载
5. **检查网络**：如果在中国大陆，可能需要等待或使用代理

**测试链接**：
- 统计卡片：https://github-readme-stats.vercel.app/api?username=HarmmerRay
- 语言统计：https://github-readme-stats.vercel.app/api/top-langs/?username=HarmmerRay

### Q: 贪吃蛇不显示？
A: 
1. **确认已创建同名仓库**：必须创建名为 `HarmmerRay` 的公开仓库
2. **检查 GitHub Actions 权限**：
   - 进入仓库 Settings → Actions → General
   - 确保 "Workflow permissions" 设置为 "Read and write permissions"
   - 勾选 "Allow GitHub Actions to create and approve pull requests"
3. **确认工作流文件存在**：检查 `.github/workflows/snake.yml` 文件是否存在
4. **手动触发工作流**：
   - 进入仓库的 Actions 标签页
   - 选择 "Generate Snake Animation" 工作流
   - 点击 "Run workflow" 手动触发
5. **检查工作流运行状态**：
   - 查看 Actions 标签页中的运行记录
   - 如果有错误，点击查看详细日志
6. **检查 output 分支**：
   - 工作流运行成功后，应该会创建 `output` 分支
   - 在 `output` 分支中应该有 `github-contribution-grid-snake.svg` 文件
7. **验证文件路径**：
   - 直接访问：https://raw.githubusercontent.com/HarmmerRay/HarmmerRay/output/github-contribution-grid-snake.svg
   - 如果 404，说明文件还未生成
8. **等待缓存更新**：首次生成后可能需要几分钟才能显示

### Q: 如何自定义贪吃蛇样式？
A: 可以在 `snake.yml` 中添加更多参数，参考 [Platane/snk](https://github.com/Platane/snk) 文档。

### Q: 工作流运行失败怎么办？
A:
1. 检查日志中的错误信息
2. 确保使用的是最新版本的 actions（已更新为 v4）
3. 确保 GITHUB_TOKEN 权限足够
4. 尝试删除并重新创建工作流文件

---

## 📚 相关资源

- [github-readme-stats](https://github.com/anuraghazra/github-readme-stats) - GitHub 统计卡片
- [Platane/snk](https://github.com/Platane/snk) - 贪吃蛇贡献图生成工具
- [GitHub Actions 文档](https://docs.github.com/en/actions)

