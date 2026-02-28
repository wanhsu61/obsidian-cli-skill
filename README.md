# Obsidian CLI Skill for OpenClaw

通过命令行操作 Obsidian 笔记，无需打开 GUI 即可管理你的知识库。

## 安装

```bash
openclaw skill install obsidian-cli
```

## 功能

- 📄 创建、读取、编辑、删除笔记
- 🔍 全文搜索与标签管理
- ✅ 任务管理（待办/已完成）
- 📅 每日笔记快捷操作
- 🔗 链接分析（入链/出链/孤立文件）
- 🏷️ YAML Frontmatter 属性管理
- 🔌 插件与主题管理

## 前置要求

1. macOS 上已安装 Obsidian App
2. 确保 `~/.local/bin` 在 PATH 中
3. Obsidian 应用正在运行

## 快速开始

```bash
# 创建笔记
obsidian create name="新想法" content="# 灵感\n\n今天想到..."

# 追加到今日笔记
obsidian daily:append content="- [ ] 完成项目报告"

# 搜索关键词
obsidian search query="AI"
```

详细用法请查看 [SKILL.md](./SKILL.md)

## License

MIT
