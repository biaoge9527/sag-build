# sag-build

自建 x86_64 版 [`Zleap-AI/SAG-Wiki`](https://github.com/Zleap-AI/SAG-Wiki) 的 `sag` 命令行。

上游只发布 `macOS-arm64` 与 `Windows-x64` 预编译包，Intel Mac 与 Linux 无官方产物。
本仓库用 GitHub Actions 从上游源码编译 `x86_64` 版本，产物放在 [Releases](../../releases)。

## 产物

| 文件 | 平台 |
| --- | --- |
| `sag-<tag>-macos-x86_64.tar.gz` | macOS Intel（x86_64，`MACOSX_DEPLOYMENT_TARGET=11.0`） |
| `sag-<tag>-linux-x86_64.tar.gz` | Linux x86_64 |

每个包附 `.sha256`。

## 安装（macOS Intel）

```bash
tar -xzf sag-<tag>-macos-x86_64.tar.gz
xattr -d com.apple.quarantine ./sag
mv sag ~/.local/bin/sag
sag --version
```

## 升级

```bash
mv ~/.local/bin/sag ~/.local/bin/sag.prev
mv sag ~/.local/bin/sag
sag --version
```

索引跨版本兼容性上游未声明，保守按「重建」处理：`sag add <目录>` 重新索引，无模型调用、无 API 费用。

## 这套流程什么时候退休

上游一旦发布 `macOS x64` 或 `Linux` 预编译包，直接改用官方包与其自带更新，本仓库停用。

## 机制

`build-sag.yml` 每天 21:00 UTC（北京 05:00）检查上游最新 release tag；
本仓库没有同名 Release 时，`git clone --depth 1 --branch <tag>` 上游源码后编译。
也可手动 `workflow_dispatch` 指定 tag 或强制重编。

不 fork 上游，避免每次升级都要合并冲突。

## 许可

上游为 MIT。本仓库仅存放构建产物与 CI 配置，源码版权归 Zleap-AI 所有。
