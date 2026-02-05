# Linux 安装使用指南

## 🐧 系统要求

- **Python**: 3.8+
- **Blender**: 2.80+ (需要系统安装的 Blender)
- **OS**: Ubuntu 18.04+, CentOS 7+, Debian 9+, 或其他 Linux 发行版

## 📦 安装步骤

### 1. 安装 Python 依赖

```bash
# 安装 customtkinter (现代化界面，强烈推荐)
pip3 install customtkinter

# 或使用系统包管理器
# Ubuntu/Debian:
sudo apt install python3-pip
pip3 install customtkinter
```

### 2. 安装 Blender

```bash
# Ubuntu/Debian (推荐方式)
# 从官网下载最新版: https://www.blender.org/download/

# 或命令行安装 (旧版)
sudo apt update
sudo apt install blender

# 验证安装
blender --version
```

### 3. 下载程序

将以下文件放在同一目录：
- `compressor_pro.py`
- `BlenderHeadlessCompressor_optimized.py`

## 🚀 运行程序

```bash
cd /path/to/compressor
python3 compressor_pro.py
```

## 📝 Linux 特别说明

### 1. 文件路径

- **Source**: Linux 路径格式 `/home/username/projects/`
- **Destination**: 同上
- **Blender**: 使用 `which blender` 找到路径，通常是 `/usr/bin/blender`

### 2. 权限问题

如果遇到权限错误：

```bash
# 给 Blender 添加执行权限 (如果是手动下载的版本)
chmod +x /path/to/blender

# 确保输出目录有写入权限
chmod 755 /path/to/output
```

### 3. 无显示环境 (SSH/远程服务器)

如果在无 GUI 环境运行，需要配置 X11 转发或使用虚拟显示：

```bash
# 方法 1: SSH X11 转发
ssh -X user@host
python3 compressor_pro.py

# 方法 2: 使用 Xvfb (虚拟显示)
sudo apt install xvfb
xvfb-run python3 compressor_pro.py
```

### 4. 缺少 tkinter

```bash
# Ubuntu/Debian
sudo apt install python3-tk

# CentOS/RHEL/Fedora
sudo yum install python3-tkinter
```

## 🎨 界面差异 (Linux vs Windows)

| 特性 | Windows | Linux |
|------|---------|-------|
| 拖放支持 | ✅ 完整支持 | ❌ 暂不支持 (windnd 库限制) |
| 现代化 UI | ✅ CustomTkinter | ✅ CustomTkinter |
| 文件对话框 | Windows 风格 | GTK/桌面环境风格 |
| 进程管理 | Terminate | SIGTERM |

## 🔧 故障排除

### 问题 1: `ModuleNotFoundError: No module named 'customtkinter'`

**解决**: 
```bash
pip3 install customtkinter
# 如果失败，使用标准 tkinter 界面 (程序会自动回退)
```

### 问题 2: `blender: command not found`

**解决**:
```bash
# 查找 Blender 路径
which blender
# 或
find /usr -name "blender" 2>/dev/null

# 如果手动安装，指定完整路径
# 例如: /home/username/blender-3.6.0-linux-x64/blender
```

### 问题 3: 界面无法显示 / `couldn't connect to display`

**解决**:
```bash
# 检查 DISPLAY 变量
echo $DISPLAY

# 如果是 SSH 连接，启用 X11 转发
ssh -Y user@host

# 或安装并使用 Xvfb
sudo apt install xvfb
xvfb-run -a python3 compressor_pro.py
```

### 问题 4: 文件选择对话框报错

**解决**:
```bash
# 安装 tk 文件对话框支持
sudo apt install python3-tk

# 或使用命令行参数直接运行 (未来版本支持)
```

### 问题 5: 权限被拒绝

**解决**:
```bash
# 检查文件权限
ls -la compressor_pro.py
# 应该显示 -rw-r--r-- 或 -rwxr-xr-x

# 添加执行权限
chmod +x compressor_pro.py

# 检查 Blender 权限
ls -la $(which blender)
```

## 💡 最佳实践

### 使用自定义 Blender 安装

如果系统 Blender 版本太旧：

```bash
# 1. 下载最新版 Blender
cd ~/Applications
wget https://download.blender.org/release/Blender3.6/blender-3.6.0-linux-x64.tar.xz
tar -xf blender-3.6.0-linux-x64.tar.xz

# 2. 在程序中使用完整路径
# Blender Executable: /home/username/Applications/blender-3.6.0-linux-x64/blender
```

### 后台运行

```bash
# 使用 nohup 在后台运行
nohup python3 compressor_pro.py > compressor.log 2>&1 &

# 查看日志
tail -f compressor.log
```

### 批量压缩 (命令行模式)

如果不使用 GUI，可以直接调用 Blender 脚本：

```bash
blender --background --python BlenderHeadlessCompressor_optimized.py -- \
  "/source/file.blend" \
  "/output/dir/" \
  "true" \
  "90"
```

参数说明：
1. `--`: 分隔符
2. 源文件路径
3. 输出目录
4. JPEG 转换 (true/false)
5. JPEG 质量 (10-100)

## 📞 获取帮助

如果遇到其他问题：

1. 查看日志输出
2. 运行 `python3 -c "import tkinter; print(tkinter.Tcl().eval('info patchlevel'))"` 检查 tkinter 版本
3. 确保 Blender 可以独立运行: `blender --background --python -c "import bpy; print('OK')"`

---
**版本**: 3.0 Pro (Linux Compatible)  
**更新日期**: 2026-02-05
