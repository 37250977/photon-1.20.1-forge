# Photon Forge Port

Photon 的 **Minecraft 1.20.1 / Forge 非官方移植版**。

> [!IMPORTANT]
> **授权状态：考察期，尚无书面授权。** 本仓库按原作者要求公开并维护，但尚未取得上游 LICENSE 要求的「Photon 2 Port 公开发布前的书面同意」。详见 [PORTING-PERMISSION.md](PORTING-PERMISSION.md)。

本仓库不是 Low-Drag-MC 官方发布物，也不代表原作者的官方支持或认可。使用、分发或二次开发前请先阅读 [LICENSE](LICENSE) 与 [PORTING-PERMISSION.md](PORTING-PERMISSION.md)。

---

## 这是什么

Photon 是 KilaBash 开发的 VFX 库（粒子 / 特效编辑器）。本仓库把它从上游的 Minecraft 1.21 / NeoForge 环境适配到 **Minecraft 1.20.1 / Forge 47.4.x / Java 17**。

**只做 1.20.1 适配，不加上游之外的功能。** 原社区移植版在移植之上额外加入的运行时能力已全部移除，相关注入点回退到上游写法：

- 整体特效变换：`IWholeEffectTransformer`、`WholeEffectRenderSpace`、`WholeFXEffectExecutor`
- GPU 模型透明排序：`ModelTriangleOrder`、`ProjectedTriangleOrder`
- 特效预热：`FXWarmup`
- `FXPostProcessPreparation`、`blade_test` 测试物品、UV 语义自检、参数修改即时重播预览

`src/main/java` 相对上游只多出 9 个 1.20.1 适配类（`MathCompat`、`RenderTargetCompat`、`HDRColorCompat`、`RegistryAwareNBTSerializable`、`BlendModeAccessor`、`ShaderInstanceAccessor`、`UIVisualLayerMixin`、`ReorderableTreeList`、`StretchedBillboardMath`）。

## 版本

| 项 | 值 |
|---|---|
| 端口版本 | `2.2.7` |
| 上游同步点 | `aac3bc1`（`v2.2.7`，2026-09-22） |
| 上一同步点 | `609a975`（`v2.2.6.a`，2026-09-07） |
| 本轮上游变更 | 31 个提交 / 96 个文件 |
| Minecraft / Forge / Java | 1.20.1 / 47.4.x / 17 |
| 依赖 | LDLib2 `2.2.41+forge.1.20.1`、KilaGraph `20.1.0.15` |

上游 `1.21` 分支本轮带来：glTF 蒙皮与动画整条链路、每粒子动画相位与可选帧混合、`PhotonMesh` 重构为索引三角形 + 三顶点流并支持动态网格注入、软粒子、模型朝向模式与 Unity ZXY 欧拉序、时间轴传输控件与编辑器快捷键、`FXRuntime` 播放速率。

逐项对应与 1.20.1 / Java 17 的等价改写（模式匹配 `switch` → if/else、`getTimer()` → `getFrameTime()`、1.21 顶点 API → 1.20.1 链式 API 等）见 [UPSTREAM-SYNC.md](UPSTREAM-SYNC.md)。

## 构建

前置：JDK 17、Forge 47.4.x、联网（首次解析依赖）。

先把依赖 jar 放进 `libs/`，文件名必须与 `build.gradle` 的坐标逐字一致：

```text
libs/ldlib2-forge-1.20.1-2.2.41+forge.1.20.1-all.jar
libs/kilagraph-forge-1.20.1-20.1.0.15.jar
```

```bash
./gradlew jar          # 产物在 build/libs/
./gradlew compileJava  # 只检查能否编译
```

**不要直接用 `build`**：它还会跑测试与校验任务，本仓库目前只验证过编译。路径含非 ASCII 字符时追加 `-Dfile.encoding=UTF-8`（否则 lombok 配置任务会抛 `MalformedInputException`）。

本仓库用于移植维护、兼容性验证与下游开发，不等同于官方发行渠道。

## 许可

本项目及其直接修改版本遵循 [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)（署名 — 非商业 — 相同方式共享）。完整条款见 [LICENSE](LICENSE)，要点：

- **署名**：再分发、修改或移植时须注明 Photon、作者 KilaBash、上游仓库与许可证链接，并说明改动。
- **非商业**：不得用于付费下载、商业整合、商业再分发、赞助/捐赠/众筹等任何直接或间接变现。
- **相同方式共享**：修改、分支与移植版本须继续使用 CC BY-NC-SA 4.0 并公开完整源码。
- **移植许可**：面向其他 Minecraft 版本公开发布移植版前，须取得原作者书面许可。
- **模组包**：非商业整合包可包含本模组；非商业 jar-in-jar 允许，但不得对本模组单独收费。
- **创作内容**：用 Photon 制作的特效、FX Pack、图表、数据包、视频等不属于本模组，不受本许可证约束。

商业授权与移植许可请联系原作者 KilaBash：`yefancy@foxmail.com`。

## 维护

维护流程、上游同步步骤与交接清单见 [MAINTENANCE.md](MAINTENANCE.md)。

## 致谢

- **源移植版**：[supermerlin204/photon-1.20.1-forge](https://github.com/supermerlin204/photon-1.20.1-forge) —— 本仓库的 1.20.1 Forge 源码基线来自该移植版，它完成了从 1.21/NeoForge 到 1.20.1/Forge 的初始移植与长期维护。
- **上游**：KilaBash 与 [Low-Drag-MC](https://github.com/Low-Drag-MC) 开发并维护 Photon。
- **依赖**：[LDLib2 Forge 移植版](https://github.com/supermerlin204/ldlib2-1.20.1-forge)、[KilaGraph Forge 移植版](https://github.com/supermerlin204/kilagraph-1.20.1-forge)。

## English

An unofficial **Minecraft 1.20.1 / Forge** port of [Photon](https://github.com/Low-Drag-MC/Photon) by KilaBash. It is a **1.20.1 adaptation only** — no features beyond upstream; the earlier community port's private additions (whole-effect transform, GPU model triangle sorting, FX warmup, `blade_test` item, UV semantics self-check, live parameter replay) have been removed along with their injection points.

- **Port version**: `2.2.7`, tracking upstream `v2.2.7` (`aac3bc1`)
- **Target**: Minecraft 1.20.1 / Forge 47.4.x / Java 17
- **Dependencies**: LDLib2 `2.2.41+forge.1.20.1`, KilaGraph `20.1.0.15` (place the jars in `libs/`)
- **Build**: `./gradlew jar` (or `compileJava`); do not run `build`, as tests and verification tasks are unverified here
- **License**: [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/) — attribution, non-commercial, share-alike; public ports to other Minecraft versions require the original author's prior written consent
- **Permission status**: probation, **no written consent yet** — see [PORTING-PERMISSION.md](PORTING-PERMISSION.md)

Source baseline: [supermerlin204/photon-1.20.1-forge](https://github.com/supermerlin204/photon-1.20.1-forge). Upstream sync details: [UPSTREAM-SYNC.md](UPSTREAM-SYNC.md). Maintenance: [MAINTENANCE.md](MAINTENANCE.md).
