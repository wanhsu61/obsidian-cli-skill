# Obsidian CLI Skill

通过命令行操作 Obsidian 笔记，无需打开 GUI 即可管理你的知识库。

## 前置要求

1. **安装 Obsidian CLI**：
   ```bash
   # macOS (Obsidian 已安装时)
   mkdir -p ~/.local/bin
   cat > ~/.local/bin/obsidian << 'EOF'
   #!/bin/bash
   exec "/Applications/Obsidian.app/Contents/MacOS/Obsidian" "$@"
   EOF
   chmod +x ~/.local/bin/obsidian
   ```

2. **确保 PATH 包含 `~/.local/bin`**

3. **Obsidian 应用正在运行**

## 使用场景

- 快速创建/编辑笔记而不打断工作流
- 批量处理笔记（搜索、替换、移动）
- 自动化日记、任务管理
- 与其他工具集成（如 AI 助手自动保存内容）

---

## 文件操作

### 创建笔记
```bash
# 简单创建
obsidian create name="新笔记" content="# 标题\n\n内容"

# 指定路径
obsidian create path="folder/note.md" content="内容"

# 使用模板
obsidian create name="会议记录" template="meeting-template"

# 覆盖已有文件
obsidian create name="临时笔记" content="..." overwrite
```

### 读取笔记
```bash
# 按文件名读取
obsidian read file="笔记名"

# 按路径读取
obsidian read path="folder/note.md"

# 读取当前活动文件
obsidian read active
```

### 追加/前置内容
```bash
# 追加到文件末尾
obsidian append file="笔记名" content="\n## 新章节\n\n内容"

# 前置到文件开头
obsidian prepend file="笔记名" content="> 重要提示\n\n"

# 不换行追加（inline）
obsidian append file="笔记名" content=" - 补充" inline
```

### 删除/移动/重命名
```bash
# 删除（移到回收站）
obsidian delete file="旧笔记"

# 永久删除
obsidian delete file="临时文件" permanent

# 移动
obsidian move file="笔记名" to="目标文件夹/"

# 重命名
obsidian rename file="旧名称" name="新名称"
```

---

## 每日笔记

```bash
# 打开今日笔记
obsidian daily

# 追加内容到今日笔记
obsidian daily:append content="- [ ] 完成项目报告"

# 前置内容
obsidian daily:prepend content="## 晨间思考\n\n"

# 读取今日笔记
obsidian daily:read

# 获取今日笔记路径
obsidian daily:path
```

---

## 搜索与导航

### 全文搜索
```bash
# 基本搜索
obsidian search query="关键词"

# 限制文件夹
obsidian search query="AI" path="Clippings"

# 只返回数量
obsidian search query="待办" total

# 带上下文搜索
obsidian search:context query="重要" limit=10
```

### 标签管理
```bash
# 列出所有标签
obsidian tags

# 带计数
obsidian tags counts

# 按使用频率排序
obsidian tags sort=count

# 查看特定文件的标签
obsidian tags file="笔记名"
```

### 链接分析
```bash
# 列出文件的出链
obsidian links file="笔记名"

# 列出反向链接（被哪些文件引用）
obsidian backlinks file="笔记名"

# 孤立文件（无入链）
obsidian orphans

# 死胡同文件（无出链）
obsidian deadends

# 未解析的链接
obsidian unresolved
```

---

## 属性（YAML Frontmatter）

```bash
# 列出所有属性
obsidian properties

# 带使用计数
obsidian properties counts

# 读取属性值
obsidian property:read name="tags" file="笔记名"

# 设置属性
obsidian property:set name="status" value="进行中" file="笔记名"

# 设置列表类型属性
obsidian property:set name="tags" value="[AI, 研究]" type=list file="笔记名"

# 删除属性
obsidian property:remove name="draft" file="笔记名"
```

---

## 任务管理

```bash
# 列出所有待办任务
obsidian tasks todo

# 列出已完成任务
obsidian tasks done

# 查看特定文件的任务
obsidian tasks file="项目笔记"

# 切换任务状态（第5行）
obsidian task file="笔记名" line=5 toggle

# 标记为完成
obsidian task file="笔记名" line=3 done

# 标记为待办
obsidian task file="笔记名" line=3 todo

# 自定义状态
obsidian task file="笔记名" line=3 status=">"
```

---

## Vault 信息

```bash
# Vault 基本信息
obsidian vault

# 列出所有 Vault
obsidian vaults

# 文件统计
obsidian files total
obsidian folders total

# 最近打开的文件
obsidian recents
```

---

## 插件与主题

```bash
# 列出已安装插件
obsidian plugins

# 列出已启用插件
obsidian plugins:enabled

# 安装社区插件
obsidian plugin:install id="dataview" enable

# 启用/禁用插件
obsidian plugin:enable id="templater"
obsidian plugin:disable id="plugin-id"

# 列出主题
obsidian themes

# 安装主题
obsidian theme:install name="Minimal" enable

# 切换主题
obsidian theme:set name="Things"
```

---

## 常用组合示例

### 快速记录想法到每日笔记
```bash
obsidian daily:append content="$(date '+%H:%M') - 刚刚想到：..."
```

### 创建带模板的会议记录
```bash
obsidian create name="会议-$(date +%Y%m%d)" template="meeting" open
```

### 批量归档已完成任务
```bash
# 查找所有完成的任务并移动到归档
for file in $(obsidian tasks done verbose | grep "^File:" | cut -d: -f2); do
    obsidian move file="$file" to="Archive/"
done
```

### 给搜索结果添加标签
```bash
obsidian search query="AI" format=json | jq -r '.[].file' | while read f; do
    obsidian property:set name="topic" value="AI" file="$f"
done
```

---

## 故障排除

| 问题 | 解决 |
|------|------|
| `command not found` | 检查 `~/.local/bin` 是否在 PATH 中 |
| `Failed to connect` | 确保 Obsidian 应用正在运行 |
| 中文文件名乱码 | 确保终端使用 UTF-8 编码 |
| 权限错误 | 检查 Vault 目录的读写权限 |

---

## 参考

- [Obsidian CLI 官方文档](https://github.com/Yakitrak/obsidian-cli)
- 完整命令列表：`obsidian help`
- 特定命令帮助：`obsidian help create`
