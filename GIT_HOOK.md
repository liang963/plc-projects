# Git 提交前钩子

Git 钩子是在某个 Git 操作前后运行的小脚本。这里的 `pre-commit` 会在每次提交前检查暂存区；如果里面有 IDE 缓存或临时文件，就取消提交。

示例脚本在 [`.githooks/pre-commit.sample`](.githooks/pre-commit.sample)。它检查的文件类型与仓库 `.gitignore` 中的忽略项对应。即使误用 `git add -f` 把缓存文件放进暂存区，钩子也会提示文件名并拦住提交。

## 在 Windows 上启用

钩子默认不会自动运行。先打开 **Git Bash**，进入仓库根目录，然后执行：

```sh
cp .githooks/pre-commit.sample .git/hooks/pre-commit
chmod +x .git/hooks/pre-commit
```

这会把示例复制到 Git 实际运行钩子的目录，并赋予执行权限。启用后，每次 `git commit` 前都会运行。要停用时，在 Git Bash 里执行：

```sh
rm .git/hooks/pre-commit
```

`.git/hooks/` 是本机仓库的目录，不会上传到 GitHub。仓库里的示例脚本会保留；其他电脑需要各自复制并启用。

## 脚本怎么工作

```sh
git diff --cached --name-only
```

列出已经暂存、准备提交的文件。

```sh
case "$path" in
  *.opt|*.log) ... ;;
esac
```

按文件名后缀筛选要拦截的文件；`|` 表示“或者”。

```sh
exit 1
```

返回非零错误码，告诉 Git 停止提交。检查通过时脚本返回 `0`，Git 继续提交。

更多钩子规则见 [Git 官方文档](https://git-scm.com/docs/githooks)。
