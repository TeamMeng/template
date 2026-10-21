# {{project-name}}

## 环境设置

### 安装 Rust

```bash
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

### 安装开发工具

- `cargo-generate`：使用 GitHub 仓库模板生成新项目
- `cargo-deny`：依赖安全、许可证与来源检查
- `typos-cli`：源码拼写检查
- `git-cliff`：基于 Conventional Commits 生成 Changelog
- `cargo-nextest`：Rust 增强测试运行器

```bash
cargo install --locked cargo-generate cargo-deny typos-cli git-cliff cargo-nextest
```

（如果已安装 [`cargo-binstall`](https://github.com/cargo-bins/cargo-binstall)，可直接免编译安装：`cargo binstall -y cargo-generate cargo-deny typos-cli git-cliff cargo-nextest`）

### 使用模板创建新项目

```bash
cargo generate TeamMeng/template
```

### 安装并启用 pre-commit

```bash
pipx install pre-commit
pre-commit install
```

## 常用命令

```bash
# 代码格式化
cargo fmt --all

# 静态检查（typos + cargo-deny + fmt + clippy）
typos
cargo deny check
cargo fmt --all -- --check
cargo lint   # = clippy --workspace --all-targets --all-features -- -D warnings

# 运行测试（cargo-nextest）
cargo nt     # = nextest run --workspace --all-features

# 运行全量检查与测试（与 CI 顺序一致）
typos && cargo deny check && cargo fmt --all -- --check && cargo lint && cargo nt

# 生成 CHANGELOG.md
git-cliff -o CHANGELOG.md
```

`cargo lint` 和 `cargo nt` 是 `.cargo/config.toml` 中定义的别名。

模板仓库本身仍带有 `{{project-name}}` 占位符，cargo 命令需在 `cargo generate` 生成的项目中执行；CI 会先归一化占位符再运行上述检查。
