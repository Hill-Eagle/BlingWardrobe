## Context

`OutfitScreen` 当前在 `loadWeatherAndClothes` 中写死固定坐标调用 `fetchWeather(lat, lon)`（本次实现默认城市改为北京），后端 `/weather` 已支持动态经纬度与高德逆地理编码。依赖 `expo-location@~19.0.8` 已安装但此前仅 import 未使用。`app.json` 当前无 expo-location 配置插件（EAS 构建时不会注入定位权限）。本设计在 proposal.md 范围内给出实现要点。

## Goals / Non-Goals

**Goals:**
- 用 `expo-location` 取实时坐标并经 `fetchWeather` 请求天气。
- 权限拒绝 / 超时 / 异常均降级到北京，且可见提示。
- 提供手动刷新入口。
- 海外空数据时给出说明。

**Non-Goals:**
- 不改造后端 `/weather`（已满足）。
- 不做全局定位状态共享（各页独立获取，本次不重构数据流）。
- 不新增用户「常用城市」偏好设置（可作为后续增强）。

## Decisions

- **取坐标封装为 `getLocation()`**：内部先 `requestForegroundPermissionsAsync`，授权后 `getCurrentPositionAsync({ accuracy: Balanced, timeout: 8000 })`，返回 `{lat, lon}`；任何失败/超时返回北京兜底坐标。选择 foreground 权限，无需 background 权限。
- **超时必设**：`getCurrentPositionAsync` 未设 `timeout` 时，无 GPS 信号设备可能永久挂起加载。设 `timeout`（建议 8s）+ try/catch 降级，确保 `setLoading(false)` 一定能执行。
- **降级提示用状态标记**：`getLocation` 额外返回 `isFallback: boolean`，`OutfitScreen` 据此在天气卡片渲染提示文字，而非静默。
- **手动刷新**：搭配页 `ScrollView` 接入 `refreshing` + `onRefresh`，回调复用 `loadWeatherAndClothes`；首次挂载仍自动加载。
- **海外处理**：`fetchWeather` 已对 `weather.error` 做空值保护；当后端返回空天气（海外）时，`OutfitScreen` 展示「仅支持国内天气」提示，不渲染错误数值。
- **配置补齐**：`app.json` 增加 `plugins: [["expo-location", { locationWhenInUsePermission: "..." }]]`；Expo Go 运行时无需此插件，EAS 构建时由插件注入 Android 权限与 iOS 用途字符串。

## Risks / Trade-offs

- [真实坐标发往远端后端] → 在「我的」页或首次进入给出隐私告知；后端地址 `8.137.218.104:8000` 为既有依赖。
- [海外/无信号区域天气缺失] → 以降级北京或「仅支持国内天气」提示兜底，接受国内优先的产品定位。
- [模拟器/Expo Go 拿不到 GPS] → 走降级路径，体验与真实设备一致，不影响联调。
- [Android 权限注入需构建验证] → EAS 构建后用 `adb shell dumpsys` 或安装包检查 `AndroidManifest` 是否含定位权限。
