# 动作识别系统（基于 MediaPipe）

本项目是一个基于 **MediaPipe Pose** 的动作识别系统，支持对视频/图像中的人体动作进行识别、可视化和简单分段。

## ✅ 功能概览

- 支持识别动作：**站立、下蹲、跳跃、投掷、移动**
- 支持处理 **图片、视频、摄像头实时流**
- 可选：输出带关键点/文字标注的视频
- 可选：开启视频分段功能（按动作段拆分出小视频）
- 可选：使用 **YOLOv8** 进行人体检测与姿态比计算（提高下蹲/跳跃判定稳定性）

---

## 📦 项目结构

```
actionClassify.py   # 主入口（包含动作识别、视频分段、演示代码）
input/              # 输入素材（可放测试视频/图片）
output/             # 处理后输出（视频、分段结果等）
```

---

## 🧩 依赖

推荐使用 Python 3.8+。

**核心依赖**（必装）
- `opencv-python`
- `mediapipe`
- `numpy`

**可选依赖**（提升识别效果）
- `ultralytics`（用于 YOLOv8，提升体态比计算）
- `Pillow`（用于在图像上绘制中文文字）

推荐安装方式（使用 pip）：

```bash
pip install opencv-python mediapipe numpy
# 可选：
pip install ultralytics pillow
```

---

## ▶️ 运行方式

### 1) 直接运行（默认配置）

在项目根目录运行：

```bash
python actionClassify.py
```

程序默认使用 `main()` 内的配置：
- `input\test.mp4` 作为输入（可更改）
- `output\test.mp4` 作为输出（可更改）
- 默认模式：`video`

### 2) 调整运行模式（图片 / 视频 / 摄像头）

打开 `actionClassify.py`，找到 `main()` 中的配置部分：

```python
MODE = "video"  # 可选: "image"、"video"、"camera"
INPUT_PATH = r"input\test.mp4"
OUTPUT_PATH = r"output\test.mp4"
```

- `image`：处理单张图像（输入：图片路径，输出：可选输出图片）
- `video`：处理视频文件
- `camera`：打开摄像头实时识别（按 `q` 退出）

### 3) 启用/关闭视频分段功能

在 `main()` 中：

```python
ENABLE_SEGMENT = True  # True：按动作分段输出视频；False：只显示/保存标注视频
SEGMENT_OUTPUT_DIR = r"output\res"
```

- 当 `ENABLE_SEGMENT=True` 时，会将连续动作段按动作类型输出到指定目录。

---

## 🎯 输出解释

- **输出视频**（保存时）会在画面上标注检测到的动作名称，示例：`动作：跳跃`。
- **分段输出**会将连续的动作段保存为单独文件，并输出分段信息。

---

## 🛠️ 如何扩展/调优

- 修改 `ActionClassifier` 中的阈值、历史帧数、YOLO检测逻辑，可优化识别性能。
- 如果需要命令行参数控制，可根据现有 `argparse` 结构补充 `parser.add_argument(...)` 并将 `main()` 里的变量改为从命令行读取。

---

## 📌 注意事项

- OpenCV 的 GUI 功能在某些服务器/无头环境下可能不可用（无法调用 `cv2.imshow`）。
- 若需要在无 GUI 环境中运行，可在 `process_video(...)` 调用中将 `show_preview=False`，或在代码中禁用窗口显示。

---

## 🧪 示例

- 将测试视频放到 `input/test.mp4`，运行 `python actionClassify.py`。
- 运行后检查 `output/` 下的输出文件以及分段目录 `output/res/`。

---

如有需要，可进一步补充 “命令行参数支持”、“批量处理多视频”、“轨迹可视化”等功能。欢迎提需求！
