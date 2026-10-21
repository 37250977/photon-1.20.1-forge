# 维护指南（Maintenance Guide）

本仓库处于**考察期**，维护者承诺在一段时间内保持可构建、跟随上游、及时回应问题。本文件说明具体怎么做，便于交接。

## 1. 考察期内的维护承诺

| 项 | 承诺 |
|---|---|
| 上游跟随 | 上游 `1.21` 分支有新提交时，评估并同步；至少每月检查一次 |
| 可构建 | 任何推送到 `main` 的提交都必须通过 `gradlew compileJava`（0 error） |
| 依赖配套 | 若 LDLib2 / KilaGraph 版本变化，三个仓库同步升级并在 README 记录 |
| 问题响应 | Issue 尽量在 7 天内给出首次回应（含「暂不处理」的明确结论） |
| 变更记录 | 每个版本在 `CHANGELOG.md` 写清「上游同步点 + 1.20.1 适配点」 |
| 授权状态 | 授权状态变化时，同步更新 `PORTING-PERMISSION.md` |

考察期结束后，按原作者意见决定继续维护、归档或转交；不要自行删除仓库。

## 2. 上游同步流程

```text
1) 记录当前同步点（README / UPSTREAM-SYNC.md 里的 commit）
2) 拉取上游并比对
3) 分类变更：新增 / 未改动 / 端口已适配 / 端口已删
4) 三方合并（ours=本仓库, base=上次同步点, theirs=上游新提交）
5) 解决冲突，逐条记录「为什么这样改」
6) gradlew clean compileJava（必要时 compileTestJava）到 0 error
7) 更新 UPSTREAM-SYNC.md 与 CHANGELOG.md
8) 提交 + 打 tag
```

关键约束：

- **只做 1.20.1 适配，不加上游之外的功能**。任何「本移植新增」的能力都应拒绝或单独讨论，避免再次偏离上游。
- 上游用 Java 21（记录模式、模式匹配 `switch`、`List.getFirst/getLast`、`Math.clamp`）与 1.20.5+ API（DataComponents）时，必须改写为 Java 17 / 1.20.1 等价实现，并记录在 `UPSTREAM-SYNC.md`。
- 加载器差异（NeoForge ↔ Forge）按语义等价实现，不要照抄类名。

## 3. 本地构建

```bash
# 依赖：JDK 17、Forge 47.4.x、联网（首次解析依赖）
# 依赖 jar 放入 libs/（文件名见 libs/README.md）

./gradlew clean compileJava
./gradlew jar        # 产物在 build/libs/
```

路径含非 ASCII 字符时追加 `-Dfile.encoding=UTF-8`（否则 lombok 配置任务会抛 `MalformedInputException`）。

## 4. 发布流程

```bash
git tag v<版本>
git push origin main --tags
```

GitHub Release 附件用 `build/libs/` 里带 `-all`（如适用）的 jar；正文可直接引用 `CHANGELOG.md` 对应段落。

## 5. 交接清单

若需转交维护，请一并移交：

- 本仓库与两个依赖仓库的 push 权限
- 原作者要求「上传并维护」的原始沟通记录（注意：目前**尚无书面授权**，状态见 `PORTING-PERMISSION.md`）
- 上游同步点的最后 commit 与下一次检查时间
- 已知未验证项（见 `UPSTREAM-SYNC.md` 的「验证」节）
