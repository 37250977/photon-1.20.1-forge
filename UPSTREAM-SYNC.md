# 上游同步记录

本仓库是 [Low-Drag-MC/Photon](https://github.com/Low-Drag-MC/Photon) 的 Minecraft 1.20.1 / Forge 移植。本文件记录「上一次同步点 → 当前同步点」，以及本轮与上游之间**必须知道的结构性差异**。

## 当前同步点

| 项 | 值 |
|---|---|
| 上游分支 | `1.21` |
| 上游目标提交 | `aac3bc1`（`v2.2.7`，2026-09-22） |
| 上游基线（上一轮同步点） | `609a975`（`v2.2.6.a`，2026-09-07） |
| 本轮上游提交数 | 31 |
| 本轮变更文件 | 96（上游新增 43 / 与基线逐字节相同 17 / 端口已适配 36） |
| 端口版本 | `2.2.7` |
| 依赖的 LDLib2 / KilaGraph | `2.2.41+forge.1.20.1` / `20.1.0.15` |
| 目标环境 | Minecraft 1.20.1 / Forge 47.4.x / Java 17 |

## 本轮带来的上游变化

- **glTF 蒙皮与动画整条链路**：读取 glTF 的 skin 与 animation 并在 CPU 上蒙皮；骨架 / 动画剪辑 / 顶点动画烘焙 / 剪辑重定向；可按时间轴摆放的「动画 glTF 模型源」；纯动画（无蒙皮）glb 与未蒙皮节点也能动；导入 rigged glb 直接作为动画源。
- **每粒子动画相位**：一群粒子不再同步动；可选帧混合（用一次纹素采样换烘焙帧）。
- **`PhotonMesh` 重构**：从「四边形 + 交错顶点」改为**索引三角形 + 三顶点流**（geometry / attributes / tangents），并新增**动态网格注入**（绘制过程中几何可变）。
- **软粒子**：纹理与 sprite 材质可选择开启。
- 模型朝向模式（对齐 Unity 的 `RenderAlignment`），粒子欧拉角改按 Unity 的 **ZXY** 序。
- 时间轴传输控件改为方形图标按钮，新增 step / 跳到结尾、编辑器快捷键（场景变换工具、场景视图开关、时间轴）。
- `FXRuntime` 新增播放速率（对象与时间轴时钟一起缩放）。
- 修复：延迟时首帧数据错误、缺失材质在 GPU 实例化下未变换、per-particle 相位在非 GPU 实例化下冻结、网格预览与发射形状画成绑定姿势等。

## 1.20.1 / Java 17 侧的适配（与上游的差异）

| 上游写法 | 本移植的写法 | 位置 |
|---|---|---|
| 模式匹配 `switch`（`case Sampler2DValue sampler -> ...`） | if/else 链，顺序与语义不变 | `RenderGraphExecutor`、`ShaderGraphMaterial` |
| `Minecraft.getTimer()`（1.21 的 DeltaTracker） | `Minecraft#getFrameTime()`（1.20.1 里等于 `timer.partialTick`，正是需要的 0..1 插值因子；不是 `getDeltaFrameTime()`） | `AnimatedGltfModelSource` |
| 1.21 顶点 API `addVertex/setColor/setUv/setLight/setNormal` | 1.20.1 链式 `vertex().color().uv().uv2().normal().endVertex()`，元素顺序遵循 `VertexFormat` | `TileParticleRenderer` |
| `LDLibFonts` 的字体访问器 | `Minecraft.getInstance().font`（与 `DrawerHelper.drawText` 同一字体） | `FXTimelineView` |
| `net.neoforged.api.distmarker` | `net.minecraftforge.api.distmarker` | 新增文件 14 个 / 28 处 |
| GL 入口（`GL_TEXTURE_BUFFER` / `glTexBuffer` / `glDrawElementsInstanced`） | LWJGL 3.3.1 下由 `GL33.*` 静态导入提供（`GL33 extends GL32 extends GL31`） | `InstancedRenderBackend` |

### `PhotonMesh` 重构

上游把 `PhotonMesh` 换成索引三角形 + 三顶点流，旧的 `quadCount()` / `vertices()` / `vertexOffset()` / `FLOATS_PER_VERTEX` / `shadeBrightness()` 全部消失。本仓库的模型几何构建按新 API 重写（`vertexCount()` / `geometry()` / `attributes()` / `tangents()` / `indices()` / `spriteBounds()` / `geometryOffset()` / `attributeOffset()` / `tangentOffset()` / `spriteOffset()`），EBO 直接上传 `mesh.indices()`，`modelEboSize = indices.length`。这部分与上游一致，没有额外的排序或分组逻辑。

## 未随上游同步的部分

- 上游 1.21 的 `26.1/26.2` 分支不在同步范围（只跟 `1.21`）。
- **社区移植版的私有功能已全部移除**：整体特效变换（`IWholeEffectTransformer` / `WholeEffectRenderSpace` / `WholeFXEffectExecutor`）、GPU 模型透明排序（`ModelTriangleOrder` / `ProjectedTriangleOrder`）、特效预热（`FXWarmup`）、`FXPostProcessPreparation`、`blade_test` 测试物品、UV 语义自检（`UvSemanticsCheck` / `UvSemanticsScenario`）。对应的注入点也回退到上游写法：`FacingOrientationHelper`、`TileParticleRenderer`、`BeamParticleRenderer`、`ParticleInstanceRenderer`、`InstancedRenderBackend`、`PhotonParticleManager`、`GameRendererMixin`、`PhotonClientProxy`。
- 参数修改即时重播预览（编辑器预览路径）同样不再包含。

> 判据：本仓库 `src/main/java` 相对上游 `aac3bc1` 只多出 1.20.1 适配类（`MathCompat`、`RenderTargetCompat`、`HDRColorCompat`、`RegistryAwareNBTSerializable`、`BlendModeAccessor`、`ShaderInstanceAccessor`、`UIVisualLayerMixin`、`ReorderableTreeList`、`StretchedBillboardMath`），没有任何上游之外的运行时能力。

## 验证

- `gradlew compileJava`：**BUILD SUCCESSFUL**（0 error）。另做 `clean compileJava` 全量复核：0 error（打包后 jar 内含 631 个 class），排除增量缓存掩盖问题的可能。
- `gradlew jar`（含 `processResources` / `addMixinsToJar` / `reobfJar`）：产出 `build/libs/photon-forge-1.20.1-2.2.7.jar`，内含 `photon.mixins.json`、refmap、蒙皮与软粒子新类。
- 剩余 16~18 条 warning 均为本轮之前就有的（Iris `@Accessor` 映射缺失、Forge 弃用 API）。
- **仅做了编译验证**：蒙皮/动画路径与顶点元素顺序都没有运行或视觉验证。
