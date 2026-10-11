## 1. 配置与依赖

- [x] 1.1 在 `BlingMirror/app.json` 的 `expo` 下新增 `plugins: [["expo-location", { "locationWhenInUsePermission": "用于获取您所在城市的实时天气与穿搭建议" }]]`，验证 JSON 合法（`npx expo config --type public` 不报错）。
- [x] 1.2 确认 `expo-location@~19.0.8` 已在 `package.json` 依赖中（已存在，仅核对版本）。

## 2. 实时定位与降级

- [x] 2.1 在 `OutfitScreen` 新增 `getLocation()`：先 `requestForegroundPermissionsAsync`，授权后 `getCurrentPositionAsync({ accuracy: Balanced, timeout: 8000 })`，返回 `{ lat, lon, isFallback }`；权限拒绝/超时/异常均返回北京兜底坐标且 `isFallback: true`。验证：模拟拒绝权限时返回北京且 `isFallback` 为 true。
- [x] 2.2 改造 `loadWeatherAndClothes`，用 `getLocation()` 取代写死坐标，并透传 `isFallback`。验证：授予权限时 `fetchWeather` 收到真实经纬度（`lon,lat` 顺序正确）。
- [x] 2.3 确保 `getCurrentPositionAsync` 设 `timeout` 后，无 GPS 信号设备不会永久卡在加载（`setLoading(false)` 必经）。

## 3. 界面提示与刷新

- [x] 3.1 天气卡片在 `isFallback` 为 true 时显示「定位失败，已使用默认城市」提示。验证：拒绝定位后卡片出现该提示而非静默。
- [x] 3.2 天气卡片在后端返回空天气（海外）时显示「仅支持国内天气」说明，不渲染错误数值。验证：构造空天气响应时显示该说明。
- [x] 3.3 为搭配页 `ScrollView` 接入 `refreshing` + `onRefresh`，复用 `loadWeatherAndClothes` 实现手动刷新，含加载反馈。验证：下拉后重新获取定位与天气。

## 4. 隐私告知

- [x] 4.1 在「我的」页或首次进入搭配页时，提供位置用途告知（文案说明用于获取天气与穿搭建议）。验证：说明文案在界面可见。

## 5. 验证与构建

- [ ] 5.1 真机/模拟器运行 `npx expo start`，进入搭配页验证：授权→真实天气；拒绝→北京降级+提示；下拉→刷新。（需设备或模拟器手动验证，本环境无法运行）
- [ ] 5.2 （可选）执行一次 `eas build` 验证 Android `AndroidManifest` 含定位权限、iOS 用途字符串注入成功。（可选，需 EAS 构建环境）
