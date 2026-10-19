# dsh-boot-animation-custom

DeepSeek Harness 的启动加载动画（定制版）。在 DSH 启动时用一段短片覆盖启动页，片尾交叉溶解进入界面；支持多片随机、固定播放某一部、在设置里直接上传/管理素材。

基于 [lxj5820/dsh-boot-animation](https://github.com/lxj5820/dsh-boot-animation)（MIT）定制，保留原作者的版权与许可。

## 安装

```powershell
dsh plugin --profile desktop add "C:\path\to\dsh-boot-animation-custom-0.3.0.tgz"
```

`--profile` 用你实际运行的那个：桌面版是 `desktop`，命令行 `dsh web` 用默认的 `web`。

装完**完整退出并重开 DSH**。客户端的模块图是启动快照，热刷新不会带上新插件。

卸载：

```powershell
dsh plugin --profile desktop remove dsh-boot-animation-custom
```

## 功能

- 启动时播放短片，片尾 2 秒交叉溶解进界面（时长可调）
- 多片随机轮播，不连续重复同一部
- 可切换为“固定播放一部”，在设置里选择
- 设置里可以：开关启动动画、调淡入时长、选进入方式、静音、点击直接进入、隐藏提示文字
- 素材池：勾选参与播放的影片，标出 `moov` 在尾部的“未优化”片子
- 直接上传影片（mp4 / webm / m4v / mov，单个 ≤ 512 MB），或打开素材文件夹手动管理
- 启动失败或超时会自动放行，不会把 DSH 卡在启动页

## 设置入口

设置 → **启动动画**（一级导航）。

如果这个入口没出现，先看浏览器控制台有没有 `boot-animation:` 开头的警告；最常见的原因是宿主版本与插件的设置接口不匹配，或 `@deepseek-ai/schemastery` 没解析到 3.18.4 以上。

## 素材

`assets/videos/` 里自带 3 段示例短片，来自上游仓库，遵循同一 MIT 许可。替换成自己的片子：删掉示例，把你的 mp4 放进去，刷新页面即可，不用重新构建。

建议 16:9、720p、时长 8–15 秒，`moov` 在文件最前（否则启动画面会黑几秒）。仓库里的 `tools/apply-faststart.mjs` 可以无损重排。

## 构建

改 `src/client.js` 后：

```sh
node build-client.mjs
```

`entry.js` 或 `cordis.patch.yml` 的改动需要重启 DSH。

## 配置项

写在 profile 的 `cordis.patch.yml` 里也可以：

```yaml
- id: boot-animation-custom
  config:
    enabled: true
    fadeMs: 2000        # 1000-5000
    enterMode: tail     # tail | end | click
    clipMode: random    # random | fixed
    fixedClip: ''       # clipMode: fixed 时的文件名
    sound: true
    clickToEnter: false
    showHint: true
    disabledClips: []
```

## 许可

MIT。原始版权归 lxj5820；本定制版保留 `LICENSE` 与署名。
