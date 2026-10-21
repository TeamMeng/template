# {{project-name}}

Rust 项目模板：edition 2024、anyhow + tracing、严格 lint、pre-commit 与完整 CI。

## 特性

- Rust 2024，默认启用 `anyhow` 错误处理与 `tracing` 日志
- `rustfmt` / `clippy` 全量检查，clippy 以 `-D warnings` 运行
- `typos` 拼写检查、`cargo-deny` 依赖与许可证审计、`cargo-nextest` 测试
- pre-commit 钩子，提交前执行与 CI 一致的检查
- Conventional Commits + `git-cliff` 生成 Changelog
- GitHub Actions：PR/main 校验，tag 自动发布 Release

## 环境要求

- Rust stable，且不低于 1.85（edition 2024 的最低版本）
- 以下工具（CI 中自动安装）：

| 工具 | 用途 |
| --- | --- |
| `cargo-generate` | 从模板生成项目 |
| `cargo-deny` | 依赖安全、许可证与来源检查 |
| `typos-cli` | 源码拼写检查 |
| `git-cliff` | 根据 Conventional Commits 生成 Changelog |
| `cargo-nextest` | 测试运行器 |

```bash
cargo install --locked cargo-generate cargo-deny typos-cli git-cliff cargo-nextest
```

可选：安装 [`cargo-binstall`](https://github.com/cargo-bins/cargo-binstall) 后免编译安装：

```bash
cargo binstall -y cargo-generate cargo-deny typos-cli git-cliff cargo-nextest
```

### pre-commit（可选）

```bash
pipx install pre-commit
pre-commit install
```

## 快速开始

```bash
cargo generate TeamMeng/template
cd <project-name>
cargo run
```

生成过程中填写 `project-name` 与 `authors` 两个变量，分别对应 crate 名和 `Cargo.toml` 的 authors 字段。

## 开发命令

| 目的 | 命令 |
| --- | --- |
| 格式化 | `cargo fmt --all` |
| 拼写检查 | `typos` |
| 依赖审计 | `cargo deny check` |
| 格式校验 | `cargo fmt --all -- --check` |
| Clippy | `cargo lint`（等价于 `clippy --workspace --all-targets --all-features -- -D warnings`） |
| 测试 | `cargo nt`（等价于 `nextest run --workspace --all-features`） |
| 全量检查 | `typos && cargo deny check && cargo fmt --all -- --check && cargo lint && cargo nt` |
| 生成 Changelog | `git-cliff -o CHANGELOG.md` |

`cargo lint` 与 `cargo nt` 定义在 `.cargo/config.toml`。

> 模板仓库的 `Cargo.toml` 保留占位符，cargo 命令需在生成后的项目中执行；CI 会先归一化占位符。

## 项目结构

| 路径 | 说明 |
| --- | --- |
| `.cargo/config.toml` | cargo 别名 |
| `scripts/cargo-hook.sh` | pre-commit 在模板仓库中跳过 cargo 检查 |
| `cliff.toml` | git-cliff 配置 |
| `deny.toml` | cargo-deny 配置 |
| `.github/workflows/build.yml` | CI 与 Release（复制到生成项目） |
| `.github/workflows/generate.yml` | 模板仓库专用的生成产物校验 |

## CI 与发布

- push 到 `main` / PR：`typos`、`cargo deny check`、`cargo fmt --check`、`clippy`、`nextest`
- 模板仓库额外校验（`generate.yml`）：`cargo generate` 后在产物中执行 `cargo fmt --check`、`clippy`、`nextest`；该 workflow 不会进入生成项目
- push tag `v*`：创建 GitHub Release，正文由 `git-cliff` 生成
- 提交信息需遵循 Conventional Commits

## License

MIT，见 [LICENSE](LICENSE)。
