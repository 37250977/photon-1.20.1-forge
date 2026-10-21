# 授权状态与派生声明（Porting Permission Notice）

本文件记录本仓库的法律状态，请在任何分发、二次开发或转交维护前先阅读。

## 1. 派生关系

本仓库是 **Photon 的 Minecraft 1.20.1 / Forge 移植版**，属于上游 [Photon 许可证](LICENSE) 中定义的 **"Photon 2 Port"**（面向 1.21.x 以外 Minecraft 版本的移植/改编）。

- 上游项目：[Low-Drag-MC/Photon](https://github.com/Low-Drag-MC/Photon)，作者 **KilaBash**
- 1.20.1 Forge 源码基线：[supermerlin204/photon-1.20.1-forge](https://github.com/supermerlin204/photon-1.20.1-forge)
- 本仓库：在该基线上同步上游 `1.21` 分支并做等价化清理（移除社区版私有功能，只保留 1.20.1 适配）

## 2. 上游许可证的要求

上游 [LICENSE](LICENSE) 的 *Restrictions on Photon 2 Ports* 第 1 条写明：

> **Prior Consent Requirement** — Any Photon 2 Port intended for public distribution or release must first obtain explicit written consent from the original author (KilaBash) prior to publication.

同时第 4 条 *Attribution and Accountability* 要求所有 Photon 2 Port 明确标注其派生状态，并对原项目与作者署名。

## 3. 当前状态（重要，请勿误读）

| 项 | 状态 |
|---|---|
| 作者是否要求本仓库公开 | 是 —— 原作者要求上传 GitHub 并维护一段时间 |
| 是否已取得**书面**授权文件 | **否** |
| 当前所处阶段 | **考察期（试用/观察阶段）** |
| 可否视为已获正式授权 | **不可以** |

也就是说：公开与维护是**按原作者要求进行**的，但尚未形成一份独立的书面授权。在取得书面确认之前，本仓库不应被当作「已获正式授权的官方移植版」，也不应据此开展商业用途。

**建议的补齐动作**（维护者自行完成，不写入代码）：

1. 保留原作者提出「上传 GitHub 并维护一段时间」这一要求的原始记录（私信/邮件/Issue 截图，含时间与上下文）；
2. 请原作者以书面形式（邮件或 Issue）确认：允许公开发布、允许的仓库地址、考察期时长、以及考察期结束后的安排；
3. 把确认结果补记到本文件第 3 节的表格里，并把状态改为「已获书面授权（日期 / 渠道）」。

## 4. 许可与限制摘要

- 本仓库整体沿用上游 **CC BY-NC-SA 4.0**（非商业 + 相同方式共享 + 署名）。
- **不得**用于付费下载、付费访问、商业整合、商业再分发、赞助/捐赠/众筹、商业服务等任何直接或间接变现。
- 非商业整合包可以包含本模组；非商业的 jar-in-jar 集成允许，但不得对本模组单独收费。
- 修改、分支与移植版本必须继续使用 CC BY-NC-SA 4.0，保留本许可证，并公开完整源代码。
- 使用 Photon 制作的特效、FX Pack、图表、配置、数据包、视频等**创作内容**不属于本模组，不受本许可证约束。

商业授权、移植许可或其他许可问题，请联系原作者 KilaBash：`yefancy@foxmail.com`。

## 5. 署名

- 原始项目与作者：**KilaBash** / [Low-Drag-MC](https://github.com/Low-Drag-MC)
- 1.20.1 Forge 源码基线：[supermerlin204/photon-1.20.1-forge](https://github.com/supermerlin204/photon-1.20.1-forge)
- 依赖的移植版：[ldlib2-1.20.1-forge](https://github.com/supermerlin204/ldlib2-1.20.1-forge)、[kilagraph-1.20.1-forge](https://github.com/supermerlin204/kilagraph-1.20.1-forge)

## 6. 变更记录

| 日期 | 变更 |
|---|---|
| 2026-10-02 | 建立本文件；记录「作者要求上传并维护、尚无书面授权、处于考察期」的状态 |
