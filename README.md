# Rust Project Template

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
