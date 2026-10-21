# liquidglass1021

浅色、双层面板的本地液态玻璃演示。首页可以切换最早初版与当前终版；两种模式都支持底部上拉或点击展开、下滑或点击关闭。

## 运行

用 Chrome / Edge 打开 `index.html`，无需安装依赖或联网。也可以在仓库目录运行：

```sh
python -m http.server 8080
```

然后打开 `http://localhost:8080/`。

## 两个模式

| 模式 | 入口 | 实现 |
| --- | --- | --- |
| 初版 · SVG | `modes/initial.html` | 首次交付的固定参数界面，无调参框；SVG 背景位移折射 |
| 终版 · WebGL2 | `modes/final.html` | 缓存底板纹理、持续 GPU 折射、可分离高斯模糊；包含调参、参数导入导出和原版 SVG 对照 |

模式可通过首页按钮或 `?mode=initial` / `?mode=final` 选择。独立打开模式页面可获得完整视口。

此前本地项目没有 Git 历史，初版根据首次交付实现与后续增量修改记录还原，不冒充当时的原始 Git 提交。终版是当前交付文件的直接副本。浏览器中保存的私人外观参数不在仓库内：如要迁移，请先在原页面导出参数 JSON，再在终版导入。

## 性能与工程记录

- `performance-lab.html`：可重复的受控滚动、缓存复用、GPU timer 和故障回退测试；需要时可导入同一套参数。
- [工程日志](docs/工程日志.txt)：各阶段工作、有效/作废结果、路线取舍和验证限制。
- [原始测量结果](docs/测量结果.json)：包括测试视口和像素比。
- [完整说明](docs/说明.txt)：交互、参数备份与测量边界。说明中的 `index.html` 指原交付入口，对应本仓库 `modes/final.html`；`baseline-svg.html` 指调参后、优化前的内部对照，不等同最早初版。

滚动时不关闭折射。当前静态底板适合纹理缓存，但缓存光栅化与原生 DOM 仍可能存在抗锯齿和亚像素差异。rAF 频率与 GPU 绘制耗时不能证明屏幕实际呈现稳定 120 FPS；压力下的合成线程同步尚无显示级轨迹验证。详细限制见日志。

## 预览

初版：

![初版](previews/initial.png)

终版（验收默认参数）：

![终版](previews/final.png)

## 来源

基于 [xiaofangstudio/liquid-glass-web](https://github.com/xiaofangstudio/liquid-glass-web) 与 [Shu Ding 的 liquid-glass](https://github.com/shuding/liquid-glass) 的 SVG 位移思路。终版 WebGL2 缓存渲染不依赖外部库。人物和内容均为虚构。

第三方声明见 [THIRD-PARTY-NOTICES.txt](THIRD-PARTY-NOTICES.txt)。未配置 GitHub Pages 或其他公开部署。
