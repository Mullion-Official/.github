<div align="center">

<img src="https://raw.githubusercontent.com/Mullion-Official/Mullion-Android-Kotlin-Compose/main/docs/images/hero.png" width="280" alt="MULLION reference app" />

# MULLION · 窗棂

**One window. Every pane. / 一樘窗，处处合缝。**

[![Android](https://img.shields.io/badge/Android-minSdk%2021-3DDC84?logo=android&logoColor=white)](https://github.com/Mullion-Official/Mullion-Android-Kotlin-Compose)
[![Kotlin](https://img.shields.io/badge/Kotlin-2.2-7F52FF?logo=kotlin&logoColor=white)](https://kotlinlang.org)
[![Compose](https://img.shields.io/badge/Compose-1.9-4285F4?logo=jetpackcompose&logoColor=white)](https://developer.android.com/jetpack/compose)
[![License](https://img.shields.io/badge/license-Apache--2.0-2563EB)](https://github.com/Mullion-Official/Mullion-Android-Kotlin-Compose/blob/main/LICENSE)
[![Components](https://img.shields.io/badge/components-26-0E9F6E)](#the-26-modules)

</div>

> 一樘窗如何分格，由窗棂决定。Mullion 是把一扇大窗分成若干玻璃格的结构件——它不发光、不抢眼，却决定了窗格如何排布、如何收束、如何避让铰链。组件库之于应用界面，正是这根窗棂：**320dp 到 1200dp 之间，一切分格由它定。**

MULLION 是一个开源的 **Android 原生自适应组件框架**。它不是把 Material 组件换个颜色，而是把宽度类、导航形态、分栏策略、折叠屏几何、窗口 inset 与明暗主题收束成一根可测试的窗棂，让业务 Screen 里不再出现 `if (width > 600)`。

## 一根窗棂，同时管五件事

| 宽度 | 宽度类 | 导航 | 主栏数 | 资源桶 |
| --- | --- | --- | --- | --- |
| 320–479dp | `Compact` | BottomBar | 1 | `values` |
| 480–599dp | `Medium` | BottomBar | 1 | `values` |
| 600–839dp | `Expanded` | Rail | 2 | `values-w600dp` |
| 840–1199dp | `Large` | Drawer | 3 | `values-w840dp` |
| 1200dp+ | `XL` | Drawer | 3 | `values-w840dp` |

- **Compose + View 双端**：`mullion-compose` 26 个组件；`mullion-view` 提供可混入既有 XML 工程的布局、玻璃 View 与 `ResourceScaler`。
- **折叠屏是几何**：`HingeInfo` / `Foldables` 计算铰链与窗格；`DockedSheet` 永不跨缝，`FoldablePane` 在窗格不足时退回单栏。
- **内容宽度优先**：`MultiPane` / `SearchLayout` 按实际约束决定是否收起预览/筛选栏，不让“窗口类到了 Large”把详情压成单字列。
- **尺寸从上下文来**：`MullionDimens.forBp()` + `mullionDimens()` 派生页面、卡片、栏距；`PaneStrategy` 的 gutter 默认自动；预览用 `MullionViewport`，不把 375 / 760 / 1180 散落在 Screen 里。
- **自适应明暗**：默认跟随系统。Compose 读 `MullionPalette` / `LocalMullionPalette`，View 读 `values` / `values-night`；不假设组件只存在于黑底。
- **零反射**：断点阈值是 `const`，解析是纯函数，R8 可以内联折叠。

## 参考应用，不是组件托盘

示例 App 是一个真实业务场景：**设备运维控制台**——工作台、设备搜索/筛选、列表-详情-预览、远程重启确认、动态时间线、运行时权限和设置。`DeviceRepository` + `ReferenceViewModel` + `StateFlow` 提供真实加载态、错误路径与选择一致性。

26 个组件的“组件柜”保留为独立入口，但仍使用同一套 `mullion-compose`。

<div align="center">
  <img src="https://raw.githubusercontent.com/Mullion-Official/Mullion-Android-Kotlin-Compose/main/docs/images/ref-light-workbench.png" width="300" alt="Light workbench" />
  <img src="https://raw.githubusercontent.com/Mullion-Official/Mullion-Android-Kotlin-Compose/main/docs/images/ref-dark-workbench.png" width="300" alt="Dark workbench" />
</div>

<div align="center">
  <sub>同一套组件代码：浅色 / 深色 · 411dp · Compose</sub>
</div>

<div align="center">
  <img src="https://raw.githubusercontent.com/Mullion-Official/Mullion-Android-Kotlin-Compose/main/docs/images/ref-light-fold.png" width="520" alt="Foldable list + detail" />
</div>

<div align="center">
  <sub>折叠屏展开 673dp：Rail + 列表/详情两栏</sub>
</div>

## 演示

- [竖版 47 秒：浅色 / 深色](https://github.com/Mullion-Official/Mullion-Android-Kotlin-Compose/blob/main/docs/media/mullion-demo.mp4)
- [大屏 52 秒：1280dp、Drawer、三栏](https://github.com/Mullion-Official/Mullion-Android-Kotlin-Compose/blob/main/docs/media/mullion-large-screen.mp4)

## JitPack

```kotlin
repositories { maven("https://jitpack.io") }

dependencies {
    implementation("com.github.Mullion-Official.Mullion-Android-Kotlin-Compose:mullion-core:1.0.0")
    implementation("com.github.Mullion-Official.Mullion-Android-Kotlin-Compose:mullion-compose:1.0.0")
    implementation("com.github.Mullion-Official.Mullion-Android-Kotlin-Compose:mullion-view:1.0.0")
}
```

`mullion-compose` / `mullion-view` transitively depend on `mullion-core`.

## 快速开始

```bash
git clone https://github.com/Mullion-Official/Mullion-Android-Kotlin-Compose.git
cd Mullion-Android-Kotlin-Compose
./gradlew :sample:installDebug
```

```kotlin
MullionTheme {                 // 跟随系统浅色 / 深色
    MullionWindowProvider {    // 真实窗口、铰链、insets
        AdaptiveScaffold(
            bottomBar = { OrbitNav(items, selected, onSelect) },
            rail = { OrbitNav(items, selected, onSelect) },
            drawer = { OrbitNav(items, selected, onSelect) },
        ) { _ ->
            MultiPane(
                list = { DeviceList() },
                detail = { DeviceDetail() },
                preview = { DevicePreview() },
            )
        }
    }
}
```

一行宽度判断都不需要出现在业务 Screen 里。

<h2 id="the-26-modules">26 个模块</h2>

- **14 个自适应**：AdaptiveScaffold、OrbitNav、MultiPane、SearchLayout、DockedSheet、FoldablePane、AdaptiveDialog、PermissionSheet、EdgeToEdgeBox、DraggableDivider、SizeClassText、WindowSizeClass、SharedAxisTransition、ResourceScaler。
- **12 个基础件**：Button、TextField、ProgressBar、Checkbox/Radio、Switch、Slider、SegmentedControl、Badge、Avatar、Card、Tabs、Snackbar。

## 仓库

| 仓库 | 内容 |
| --- | --- |
| [Mullion-Android-Kotlin-Compose](https://github.com/Mullion-Official/Mullion-Android-Kotlin-Compose) | 框架源码、参考应用、26 组件柜、文档与演示视频 |
| [.github](https://github.com/Mullion-Official/.github) | 组织主页与公开资料 |

- [架构说明](https://github.com/Mullion-Official/Mullion-Android-Kotlin-Compose/blob/main/docs/architecture.md)
- [断点表](https://github.com/Mullion-Official/Mullion-Android-Kotlin-Compose/blob/main/docs/breakpoints.md)
- [折叠屏](https://github.com/Mullion-Official/Mullion-Android-Kotlin-Compose/blob/main/docs/foldables.md)
- [验证记录](https://github.com/Mullion-Official/Mullion-Android-Kotlin-Compose/blob/main/docs/testing.md)
- [静态文档](https://mullion-official.github.io/Mullion-Android-Kotlin-Compose/)

## 状态

当前版本 **1.0.0**：核心 33 个单元测试、参考应用 5 个 ViewModel 测试；已在 Pixel 7 与 7.6" Fold-in AVD（Android 16 / API 36）上验证 411 / 673 / 914 / 1280dp 与折叠姿态 1–3。发布坐标已准备好 Maven Central，但仓库不虚报外部发布状态。

**Apache-2.0** · 26 components · Compose + View · minSdk 21

<div align="center"><sub>MULLION · 一樘窗，处处合缝。</sub></div>
