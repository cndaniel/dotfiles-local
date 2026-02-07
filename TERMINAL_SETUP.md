# 终端配置指南（新手友好 / 多 AI CLI / Warp 友好）

这套配置基于 `~/dotfiles + ~/dotfiles-local`，目标是：
- 默认稳定，不干扰现有工作流。
- 对 `opencode`、`amp`、`codex`、`claude` 提供统一入口。
- 在 iTerm 等传统终端自动进入 tmux，在 Warp/VSCode/JetBrains 终端默认不自动接管。

## 1. 推荐安装

```bash
brew install starship zoxide eza atuin direnv
```

已保留现有工具：`bat`、`fd`、`fzf`、`ripgrep`、`tmux`。

## 2. 一次性生成补全

```bash
opencode completion zsh > ~/.zsh/completion/_opencode
codex completion zsh > ~/.zsh/completion/_codex
```

## 3. 首次启用

```bash
source ~/.zshrc
```

如果你用 `tmux`，建议再执行一次：

```bash
tmux source-file ~/.tmux.conf
```

## 4. 日常使用

- 统一 AI 入口：`ai [opencode|amp|codex|claude] [args...]`
- 默认 AI 工具：`AI_DEFAULT_TOOL=opencode`
- 常用快捷：
  - `oc` / `ocn` (`opencode`)
  - `ap` (`amp`)
  - `cc` (`claude`)
  - `cx` (`codex`)
- 目录跳转（安装 zoxide 后）：`z <目录关键词>`
- 历史增强（安装 atuin 后）：`Ctrl-r`

## 5. tmux 自动接管规则

满足以下条件时自动 attach：
- 交互式 shell 且有 TTY
- 已安装 tmux
- 当前不在 tmux 中
- 没有设置 `NO_AUTO_TMUX`
- 终端不是 Warp/VSCode/JetBrains

自动创建会话窗口：
- `main`（默认只开一个窗口，最简上手）

会话命名规则：
- 在 Git 仓库内：使用仓库目录名（例如 `dotfiles-local`）
- 在非仓库目录：使用当前目录名
- 在家目录：固定为 `home`

## 6. 常见操作

临时关闭自动 tmux：

```bash
NO_AUTO_TMUX=1 zsh
```

永久关闭自动 tmux（当前会话）：

```bash
export NO_AUTO_TMUX=1
```

切换默认 AI 工具：

```bash
export AI_DEFAULT_TOOL=codex
```

## 7. 故障排查

`ai` 提示命令不存在：
- 先执行 `command -v opencode amp codex claude`
- 确认对应工具安装并在 `PATH` 中

`starship/zoxide/atuin/direnv` 没生效：
- 先确认安装：`command -v starship zoxide atuin direnv`
- 重新加载：`source ~/.zshrc`

tmux 插件未加载：
- 配置已改成“存在才加载 TPM”
- 如需安装 TPM：
  - `git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm`

status 栏显示不喜欢：
- 当前默认显示：`会话名 | 窗口名 | 时间`
- 会话名来自上面的命名规则，不再是 `daniel__users` 这类组合名

## 8. 回滚策略

如需快速回滚，可恢复这几个文件的旧版本：
- `~/dotfiles-local/zshrc.local`
- `~/dotfiles-local/aliases.local`
- `~/dotfiles-local/tmux.conf.local`

当前目录里已经有备份文件（`*.bak-*`）可直接参考。
