# Blender Batch Compressor Pro

功能强大的 Blender 文件批量压缩工具，支持拖放、实时队列、质量调节和中断控制。

[linux指南](https://github.com/lonely814/Blender-Batch-Compressor-Pro/blob/main/README_Linux.md)

## ✨ 新特性（Pro 版本）

### 🎯 核心功能

| 功能           | 描述                                       |
| -------------- | ------------------------------------------ |
| **拖放支持**   | 直接拖拽文件夹到界面快速添加               |
| **文件队列**   | 实时显示待处理文件列表，支持大列表虚拟滚动 |
| **质量调节**   | JPEG 压缩质量滑块 (10-100%)                |
| **中断控制**   | 随时停止处理，安全退出                     |
| **单文件模式** | 逐个处理文件，实时更新进度                 |
| **详细统计**   | 每个文件的压缩比例和节省空间               |

### 🎨 界面特性

- **双版本 UI**: CustomTkinter (现代化) / 标准 Tkinter (兼容性)
- **暗色主题**: Blender 风格橙色配色
- **实时日志**: 带时间戳的详细控制台输出
- **进度追踪**: 百分比进度条和文件计数
- **可调整布局**: 可拖拽调整左右面板宽度

## 📦 文件结构

```
blender-compressor-pro/
├── compressor_pro.py                      # 主程序 (GUI)
├── BlenderHeadlessCompressor_optimized.py # Blender 处理脚本
├── README.md                              # 本文件
└── dirs.pkl                               # 自动生成的配置文件
```

## 🚀 快速开始

### 安装依赖

```bash
# 必需
pip install customtkinter   # 现代化界面 (强烈推荐)
pip install windnd          # Windows 拖放支持 (可选)
```

### 运行程序

```bash
python compressor_pro.py
```

### 使用步骤

1. **设置路径**（3种方式）
   - 点击 Browse 按钮选择
   - 直接输入路径
   - 🆕 **拖拽文件夹**到 Source 输入框

2. **扫描文件**
   - 点击 "🔍 Scan Source Directory"
   - 右侧队列面板显示所有找到的 .blend 文件

3. **配置选项**
   - ☑️ **Convert to JPEG**: 启用图片压缩
   - 🎚️ **Quality**: 拖动滑块设置 JPEG 质量 (推荐 85-95)

4. **开始压缩**
   - 点击 "🚀 Start" 开始处理
   - 实时查看进度和日志
   - 随时点击 "⏹ Stop" 安全中断

## 📊 界面说明

```
┌──────────────────────────────────────┬──────────────────────┐
│  🗜️ Blender Batch Compressor         │ 📦 File Queue         │
│                                      │                      │
│  📁 Source: [____________] [Browse]  │ 1. scene.blend       │
│  📂 Destination: [________] [Browse] │ 2. character.blend   │
│  🎨 Blender.exe: [________] [Browse] │ 3. props.blend       │
│                                      │ ...                  │
│  Options:                            │                      │
│  ☑️ Convert images to JPEG           │ [🔍 Scan Directory]  │
│  Quality: [========|====] 90%       │ Total: 15 files      │
│                                      │ Est: 1,240 MB        │
│  [🚀 Start] [⏹ Stop]                │                      │
│                                      │                      │
│  Status: Processing...               │                      │
│  [████████░░░░░░░░░░] 45%           │                      │
│  Progress: 7/15 files                │                      │
│                                      │                      │
│  Console Output:                     │                      │
│  [14:32:01] ℹ️ Loading scene.blend   │                      │
│  [14:32:03] ✅ Packed 5 files        │                      │
│  [14:32:05] ✅ Converted texture.png │                      │
│  [14:32:06] ✅ Saved 45.2 MB (63%)  │                      │
└──────────────────────────────────────┴──────────────────────┘
```

## ⚙️ 工作原理

### 处理流程

```
1. 扫描阶段
   └─ 递归遍历源目录，收集所有 .blend 文件
   └─ 计算总大小，显示在队列面板

2. 压缩阶段（逐个文件）
   ├─ 打开 .blend 文件
   ├─ Pack All: 打包所有外部资源
   ├─ JPEG Convert: 转换贴图格式（可选）
   │   ├─ 解包图片
   │   ├─ 渲染为 JPEG（指定质量）
   │   ├─ 重新加载并打包
   │   └─ 清理临时文件
   └─ Save: 使用 Blender 压缩格式保存

3. 中断处理
   └─ 完成当前文件后停止
   └─ 或强制终止 Blender 进程
```

### 技术改进

| 原版问题           | Pro 版解决方案                |
| ------------------ | ----------------------------- |
| 命令行参数传递错误 | 使用 `--` 分隔符正确传递参数  |
| 图片压缩逻辑混乱   | 使用 `save_render` + 临时文件 |
| 无法中断处理       | 多线程 + 信号处理 + 进程终止  |
| 进度未知           | 逐文件处理，实时更新进度条    |
| JPEG 质量固定      | 可配置质量滑块 (10-100%)      |

## 🎛️ 高级配置

### JPEG 质量建议

| 质量设置 | 适用场景           | 文件大小   |
| -------- | ------------------ | ---------- |
| 95-100%  | 最终渲染、重要项目 | 较大       |
| 85-95%   | 日常工作、预览     | 中等 ⭐推荐 |
| 70-85%   | 草稿、测试         | 较小       |
| 50-70%   | 缩略图、草稿       | 最小       |

### 性能优化

- **大项目**: 建议分批处理，避免内存不足
- **网络驱动器**: 慢速磁盘建议使用本地缓存
- **CPU 使用**: Blender 压缩时会占用 CPU，可调整优先级

## 🐛 故障排除

### 常见问题

**Q: 拖放功能不工作？**  
A: 安装 `pip install windnd`，或确保使用 Windows 系统

**Q: 界面显示异常？**  
A: 程序会自动回退到标准 tkinter，或手动安装 `pip install customtkinter`

**Q: 压缩后文件变大？**  
A: 可能是:

- JPEG 质量设置过高 (95%+)
- 原始文件已经压缩过
- 包含大量几何体数据（贴图压缩不影响模型）

**Q: 中断后文件损坏？**  
A: 程序会在完整处理完当前文件后才停止，不会损坏文件。目标文件会有 `_compressed` 后缀。

**Q: 某些图片转换失败？**  
A: 检查:

- 图片是否已损坏
- 是否为特殊格式（HDR, EXR 等可能需要额外处理）
- Blender 控制台输出详细错误

### 错误代码

| 退出码 | 含义       |
| ------ | ---------- |
| 0      | 成功       |
| 1      | 处理失败   |
| 2      | 被用户中断 |
| -1     | 未知错误   |

## 📝 命令行使用（高级）

也可以直接调用 Blender 脚本进行单文件处理：

```bash
blender.exe --background --python BlenderHeadlessCompressor_optimized.py -- \
  "source.blend" \
  "output_dir/" \
  "true" \
  "90"
```

参数说明:

1. `--`: 分隔符，之后是脚本参数
2. `source.blend`: 源文件路径
3. `output_dir/`: 输出目录
4. `true`: 启用 JPEG 转换 (true/false)
5. `90`: JPEG 质量 (10-100)

## 🔧 开发计划

- [ ] 多线程并行处理（多个 Blender 实例）
- [ ] 压缩预设保存/加载
- [ ] 文件过滤（按大小、日期）
- [ ] 压缩历史记录
- [ ] 命令行批量模式

## 📄 许可证

MIT License - 自由使用和修改

## 🙏 致谢

- 原始版本作者
- CustomTkinter 项目
- Blender Foundation

---

**版本**: 3.0 Pro  
**更新日期**: 2026-02-05  
**兼容性**: Python 3.8+, Blender 2.80+
