# TermAgent TODO 清单

## 概览

| 优先级 | 数量 | 说明 |
|--------|------|------|
| P0 | 12 | 必须完成，Phase 1 核心功能 |
| P1 | 8 | 应该完成，完善用户体验 |
| P2 | 5 | 可以完成，界面优化 |

---

## M1 - 基础框架搭建

### P0

- [ ] **TASK-001: 初始化 Tauri + SolidJS 项目**
  - 验收标准：
    - [ ] `npm create tauri-app` 成功创建项目
    - [ ] 选用 SolidJS + TypeScript 模板
    - [ ] 项目可 `npm run dev` 启动
    - [ ] Tauri 窗口可正常显示
  - 预估时间：1h
  - 依赖：无

- [ ] **TASK-002: 配置 Tailwind CSS**
  - 验收标准：
    - [ ] Tailwind CSS 安装成功
    - [ ] `tailwind.config.js` 正确配置
    - [ ] 全局 CSS 可使用 Tailwind 指令
    - [ ] 深色/浅色主题变量定义完成
  - 预估时间：0.5h
  - 依赖：TASK-001

- [ ] **TASK-003: 配置 TypeScript 和代码规范**
  - 验收标准：
    - [ ] `tsconfig.json` 正确配置
    - [ ] ESLint + Prettier 配置完成
    - [ ] 路径别名 `@/` 配置
  - 预估时间：0.5h
  - 依赖：TASK-001

---

## M2 - 终端仿真核心

### P0

- [ ] **TASK-004: Rust PTY 基础功能**
  - 验收标准：
    - [ ] `portable-pty` 依赖添加
    - [ ] 创建 Shell 进程功能实现
    - [ ] 写入输入功能实现
    - [ ] 关闭 Shell 功能实现
    - [ ] Windows/Linux/macOS 均可编译通过
  - 预估时间：4h
  - 依赖：TASK-001

- [ ] **TASK-005: Tauri 命令导出**
  - 验收标准：
    - [ ] `create_shell` 命令可用
    - [ ] `write_shell` 命令可用
    - [ ] `close_shell` 命令可用
    - [ ] `signal_shell` 命令可用（SIGINT/SIGTERM/SIGKILL/SIGTSTP/SIGCONT）
    - [ ] `get_default_shell` 命令可用
  - 预估时间：2h
  - 依赖：TASK-004

- [ ] **TASK-006: Shell 输出事件推送**
  - 验收标准：
    - [ ] PTY 输出通过 Tauri 事件推送
    - [ ] 前端可订阅 `shell-output` 事件
    - [ ] 支持多会话并发
  - 预估时间：2h
  - 依赖：TASK-005

- [ ] **TASK-007: xterm.js 集成**
  - 验收标准：
    - [ ] xterm.js 安装成功
    - [ ] @xterm/addon-fit 安装成功
    - [ ] 基础终端渲染正常
    - [ ] 终端尺寸自适应容器
  - 预估时间：2h
  - 依赖：TASK-002

- [ ] **TASK-008: Terminal 组件封装**
  - 验收标准：
    - [ ] TerminalSession 组件封装完成
    - [ ] 输入自动发送到对应 Shell 会话
    - [ ] 输出正确渲染在 xterm.js
    - [ ] 滚动和选择功能正常
  - 预估时间：3h
  - 依赖：TASK-006, TASK-007

---

## M3 - 会话管理

### P0

- [ ] **TASK-009: TabBar 组件开发**
  - 验收标准：
    - [ ] 显示所有标签页
    - [ ] 显示当前活动标签
    - [ ] 显示标签标题（工作目录或 Shell 名称）
    - [ ] 关闭按钮正常
    - [ ] 新建标签按钮正常
  - 预估时间：2h
  - 依赖：TASK-008

- [ ] **TASK-010: 会话状态 Store**
  - 验收标准：
    - [ ] sessionStore 实现
    - [ ] createTab / closeTab / setActiveTab 功能正常
    - [ ] 拖拽排序功能正常
  - 预估时间：2h
  - 依赖：TASK-009

- [ ] **TASK-011: 会话持久化**
  - 验收标准：
    - [ ] 退出时保存会话状态到 session.json
    - [ ] 启动时恢复会话状态
    - [ ] 每个标签页恢复工作目录和 Shell 进程
  - 预估时间：3h
  - 依赖：TASK-010

- [ ] **TASK-012: TabBar 快捷键绑定**
  - 验收标准：
    - [ ] Ctrl+T 新建标签页
    - [ ] Ctrl+W 关闭当前标签页
    - [ ] Ctrl+Tab 切换到下一个
    - [ ] Ctrl+Shift+Tab 切换到上一个
  - 预估时间：1h
  - 依赖：TASK-010

---

## M4 - 终端快捷键

### P0

- [ ] **TASK-013: 终端快捷键实现**
  - 验收标准：
    - [ ] Ctrl+C 中断进程/取消输入
    - [ ] Ctrl+D EOF（关闭 Shell）
    - [ ] Ctrl+Z 挂起进程
    - [ ] Ctrl+S 暂停输出
    - [ ] Ctrl+Q 恢复输出
    - [ ] Ctrl+L 清屏
  - 预估时间：2h
  - 依赖：TASK-008

- [ ] **TASK-014: 关闭标签确认对话框**
  - 验收标准：
    - [ ] 检测 Shell 进程是否在运行
    - [ ] 运行中时显示确认对话框
    - [ ] 用户确认后发送 SIGKILL 关闭
  - 预估时间：1h
  - 依赖：TASK-011

### P1

- [ ] **TASK-015: Ctrl+R 历史搜索**
  - 验收标准：
    - [ ] Ctrl+R 打开反向增量搜索
    - [ ] 输入关键字过滤历史命令
    - [ ] 上下箭头选择匹配项
    - [ ] 回车执行选中命令
  - 预估时间：3h
  - 依赖：TASK-013, TASK-017

---

## M5 - 命令历史

### P0

- [ ] **TASK-016: historyService 实现**
  - 验收标准：
    - [ ] 添加历史记录功能
    - [ ] 搜索历史功能
    - [ ] 获取最近 N 条功能
    - [ ] 清空历史功能
  - 预估时间：2h
  - 依赖：TASK-001

- [ ] **TASK-017: 命令历史持久化**
  - 验收标准：
    - [ ] 历史记录存储到 history.json
    - [ ] 跨会话保留历史
    - [ ] 支持按日期和内容搜索
  - 预估时间：2h
  - 依赖：TASK-016

---

## M6 - 界面定制

### P0

- [ ] **TASK-018: 主题切换功能**
  - 验收标准：
    - [ ] 深色/浅色主题切换
    - [ ] 主题即时生效，无需重启
    - [ ] xterm.js 主题同步切换
  - 预估时间：2h
  - 依赖：TASK-002

- [ ] **TASK-019: 字体大小调整**
  - 验收标准：
    - [ ] Ctrl++ 放大字体
    - [ ] Ctrl+- 缩小字体
    - [ ] Ctrl+0 重置字体大小
    - [ ] 字体大小持久化
  - 预估时间：1h
  - 依赖：TASK-002

### P1

- [ ] **TASK-020: 字体选择功能**
  - 验收标准：
    - [ ] 设置页面可选择等宽字体
    - [ ] 支持加载系统字体
    - [ ] 字体选择持久化
  - 预估时间：2h
  - 依赖：TASK-019

---

## M7 - 配置管理

### P0

- [ ] **TASK-021: UserConfig 数据模型**
  - 验收标准：
    - [ ] UserConfig 接口定义
    - [ ] 默认值配置
  - 预估时间：0.5h
  - 依赖：TASK-001

- [ ] **TASK-022: 配置持久化**
  - 验收标准：
    - [ ] 配置保存到 config.json
    - [ ] 启动时加载配置
    - [ ] 配置更新即时生效
  - 预估时间：1h
  - 依赖：TASK-021

- [ ] **TASK-023: 设置面板 UI**
  - 验收标准：
    - [ ] 主题选择组件
    - [ ] 字体大小调节组件
    - [ ] 字体选择组件
    - [ ] Shell 路径配置组件
  - 预估时间：3h
  - 依赖：TASK-022

---

## M8 - Shell 检测与回退

### P0

- [ ] **TASK-024: 系统默认 Shell 检测**
  - 验收标准：
    - [ ] Windows: 检测 PowerShell/CMD/Git Bash/WSL
    - [ ] macOS: 检测 zsh/bash
    - [ ] Linux: 检测 zsh/bash/dash
    - [ ] 返回第一个存在的 Shell
  - 预估时间：2h
  - 依赖：TASK-005

- [ ] **TASK-025: Shell 不存在回退**
  - 验收标准：
    - [ ] 指定 Shell 不存在时回退到默认
    - [ ] 显示提示信息
    - [ ] Shell 启动失败显示错误信息
  - 预估时间：1h
  - 依赖：TASK-024

---

## M9 - 复制粘贴

### P1

- [ ] **TASK-026: 复制粘贴支持**
  - 验收标准：
    - [ ] Ctrl+Shift+C 复制选中内容
    - [ ] Ctrl+Shift+V 粘贴
    - [ ] 鼠标右键菜单复制粘贴
  - 预估时间：2h
  - 依赖：TASK-007

- [ ] **TASK-027: 文本选择**
  - 验收标准：
    - [ ] 鼠标选中文字
    - [ ] 双击选中整行
    - [ ] 选择样式清晰可见
  - 预估时间：1h
  - 依赖：TASK-007

---

## M10 - 滚动回溯

### P1

- [ ] **TASK-028: 滚动回溯功能**
  - 验收标准：
    - [ ] 鼠标滚轮滚动历史输出
    - [ ] PageUp/PageDown 浏览历史
    - [ ] 自动滚动到最新输出
  - 预估时间：1h
  - 依赖：TASK-007

---

## M11 - Unicode 和高级终端支持

### P1

- [ ] **TASK-029: Unicode 完整支持**
  - 验收标准：
    - [ ] emoji 正常显示
    - [ ] 双宽字符正确渲染
    - [ ] CJK 字符正常显示
  - 预估时间：1h
  - 依赖：TASK-007

- [ ] **TASK-030: SSH/tmux/screen 兼容**
  - 验收标准：
    - [ ] SSH 连接正常建立
    - [ ] tmux 会话可创建和使用
    - [ ] screen 会话可创建和使用
  - 预估时间：3h
  - 依赖：TASK-004

---

## M12 - 构建发布

### P0

- [ ] **TASK-031: Windows 构建配置**
  - 验收标准：
    - [ ] `npm run tauri build` 成功
    - [ ] 生成 .msi 安装包
    - [ ] 安装后可正常运行
  - 预估时间：1h
  - 依赖：所有 P0 任务完成

### P2

- [ ] **TASK-032: macOS 构建配置**
  - 验收标准：
    - [ ] macOS 下可构建
    - [ ] 生成 .app Bundle
  - 预估时间：1h
  - 依赖：TASK-031

- [ ] **TASK-033: Linux 构建配置**
  - 验收标准：
    - [ ] Linux 下可构建
    - [ ] 生成 .AppImage
  - 预估时间：1h
  - 依赖：TASK-031

---

## 任务依赖图

```
TASK-001 (项目初始化)
  ├── TASK-002 (Tailwind)
  ├── TASK-003 (TypeScript)
  ├── TASK-004 (Rust PTY) ← TASK-001
  │     └── TASK-005 (Tauri 命令) ← TASK-004
  │           ├── TASK-006 (事件推送) ← TASK-005
  │           │     └── TASK-008 (Terminal组件) ← TASK-006, TASK-007
  │           │           ├── TASK-009 (TabBar) ← TASK-008
  │           │           │     └── TASK-010 (sessionStore) ← TASK-009
  │           │           │           ├── TASK-011 (会话持久化) ← TASK-010
  │           │           │           ├── TASK-012 (TabBar快捷键) ← TASK-010
  │           │           │           └── TASK-014 (关闭确认) ← TASK-011
  │           │           └── TASK-013 (终端快捷键) ← TASK-008
  │           │                 └── TASK-015 (历史搜索) ← TASK-013, TASK-017
  │           └── TASK-024 (Shell检测) ← TASK-005
  │                 └── TASK-025 (回退) ← TASK-024
  └── TASK-016 (historyService) ← TASK-001
        └── TASK-017 (历史持久化) ← TASK-016

TASK-007 (xterm.js) ← TASK-002
  ├── TASK-026 (复制粘贴) ← TASK-007
  ├── TASK-027 (文本选择) ← TASK-007
  └── TASK-028 (滚动回溯) ← TASK-007
        └── TASK-029 (Unicode) ← TASK-007
              └── TASK-030 (SSH/tmux) ← TASK-004

TASK-018 (主题切换) ← TASK-002
  └── TASK-019 (字体大小) ← TASK-002
        └── TASK-020 (字体选择) ← TASK-019

TASK-021 (UserConfig) ← TASK-001
  └── TASK-022 (配置持久化) ← TASK-021
        └── TASK-023 (设置面板) ← TASK-022
```

---

## 里程碑

### M1 - 基础框架 (Week 1)
**目标**：项目可运行，基础架构完成
**交付物**：TASK-001, TASK-002, TASK-003, TASK-004

### M2 - 终端核心 (Week 2)
**目标**：终端渲染和 Shell 管理完成
**交付物**：TASK-005, TASK-006, TASK-007, TASK-008, TASK-013, TASK-024, TASK-025

### M3 - 会话管理 (Week 3)
**目标**：多标签页和会话持久化完成
**交付物**：TASK-009, TASK-010, TASK-011, TASK-012, TASK-014

### M4 - 历史和配置 (Week 4)
**目标**：命令历史和用户配置完成
**交付物**：TASK-015, TASK-016, TASK-017, TASK-018, TASK-019, TASK-020, TASK-021, TASK-022, TASK-023

### M5 - 完善和发布 (Week 5)
**目标**：通过验收测试，可发布
**交付物**：TASK-026, TASK-027, TASK-028, TASK-029, TASK-030, TASK-031, TASK-032, TASK-033
