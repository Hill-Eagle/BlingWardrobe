## Why

当前「搭配」页的天气坐标写死为固定默认值（本次实现将默认降级城市改为北京，39.90, 116.40），无法反映用户真实所在地，导致穿搭建议与实际天气脱节。此前的一次实现尝试还暴露出稳健性缺口：定位无超时可能卡死加载、降级静默无提示、仅挂载时取一次无法刷新、海外坐标下高德无数据反而比写死更差。本变更把天气改为实时定位，并补齐上述稳健性与体验遗漏。

## What Changes

- 将「搭配」页天气从写死坐标改为调用 `expo-location` 获取设备实时经纬度。
- 定位权限被拒、获取异常或超时（新增 `timeout`）时，降级到默认城市（北京，39.90, 116.40）。
- 降级生效时在天气卡片明确提示「定位失败，已使用默认城市」，避免用户误以为天气准确。
- 新增手动刷新天气入口（搭配页下拉刷新或按钮），解决冷启动 GPS 未就绪与换城市不更新的问题。
- 后端 `/weather`（高德）对海外/非覆盖区返回空数据时，前端提示「仅支持国内天气」，而非默默无卡片。
- `app.json` 增加 `expo-location` 配置插件与 iOS 定位用途描述字符串（用于 EAS 构建注入权限）。
- 坐标顺序已核对正确：`getLocation()` 返回 `{lat, lon}` → `fetchWeather(lat, lon)` → 后端 `location={lon},{lat}`，与高德要求一致，无需改动。

## Capabilities

### New Capabilities
- `weather-location`: 天气获取基于设备实时定位，含权限处理、超时降级、降级提示与手动刷新等行为。

### Modified Capabilities
<!-- 无既有 spec 需修改；项目当前 specs 为空。 -->

## Impact

- 受影响代码：`BlingMirror/App.tsx`（`OutfitScreen`：`getLocation`、天气卡片、刷新）、`BlingMirror/app.json`（expo-location 插件与 iOS 用途描述）。
- 依赖：`expo-location@~19.0.8` 已存在于 `package.json`，无需新增依赖。
- 后端：`BlingMirror-backend/main.py` 的 `/weather` 已支持动态经纬度，本次不改动；海外空数据由前端处理。
- 隐私：真实坐标将发送至远端后端 `8.137.218.104:8000`，需在「我的」或首次提示中告知用户。
- 构建：EAS 构建需 `app.json` 插件在 prebuild 阶段注入 Android 定位权限与 iOS 用途字符串。
