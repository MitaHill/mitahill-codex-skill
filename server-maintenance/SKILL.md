---
name: server-maintenance
description: 服务器维护 — 环境探查、问题诊断、最小改动修复
disable-model-invocation: true
---

# Server Maintenance

## 1. 环境探查（无上下文时）

当没有明确的上下文信息时，先执行全面的环境探查：

```bash
# 系统信息
uname -a
cat /etc/os-release 2>/dev/null || cat /etc/*release 2>/dev/null

# PCI 设备
lspci 2>/dev/null || cat /proc/bus/pci/devices 2>/dev/null

# GPU 状态
nvidia-smi 2>/dev/null || echo "NVIDIA GPU not found"
# AMD GPU
rocm-smi 2>/dev/null || cat /sys/class/drm/*/device/vendor 2>/dev/null

# 资源使用
free -h
df -h
ip -s link 2>/dev/null || ifconfig 2>/dev/null

# 目录概览
ls -la /root/

# Docker
docker ps -a 2>/dev/null || echo "Docker not available"
```

将探查结果汇总后呈现给用户。

## 2. 问题诊断流程

### 2.1 联网搜索

用户提出问题后，首先联网搜索可能的解决方案。搜索时要明确关键词，包括：
- 操作系统版本
- 软件/服务名称和版本
- 错误信息关键字
- 已知的兼容性问题

### 2.2 只读调查

以只读方式调查目标系统或服务的详细信息：

- 查看配置文件（不修改）
- 查看日志文件
- 查看进程状态
- 查看服务状态
- 查看网络连接状态

### 2.3 报告症状

**不要直接进行任何修改操作。** 将以下信息完整呈现给用户：

1. **症状描述** — 当前系统表现出的问题和现象
2. **调查详情** — 配置文件内容、日志片段、状态信息等
3. **根因分析** — 判断可能的原因
4. **解决方案** — 列出一种或多种可行的修复方案，说明每种方案的优缺点和影响范围

### 2.4 等待用户决策

用户确认方案后，再开始操作。

## 3. 修复执行

- 遵循 **最小改动原则** — 只修改必要的部分，不做额外"优化"
- **稳健操作** — 每一步都确保可回退（备份配置文件、记录原始状态）
- **一次只改一个点** — 修改后确认效果，再进行下一步
- **不要频繁变换思路** — 确定方案后一以贯之，避免中途切换方案

## 4. 修复验证

- 确认问题已解决
- 检查相关服务运行正常
- 确认系统关键指标无异常变化
- 总结做了哪些修改，以及回退方法
