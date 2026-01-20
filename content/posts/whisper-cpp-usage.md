+++
date = '2026-01-20T19:06:09+08:00'
draft = false
title = 'whisper.cpp 使用记录'
description = '使用 whisper.cpp 生成音频/视频字幕'
+++

## 文件夹

```
[hikari@lenovo whisper_cpp]$ tree
.
├── audios
│   ├── test_1.wav
│   └── test_2.wav
├── models
│   ├── ggml-base.bin
│   ├── ggml-large-v3.bin
│   ├── ggml-large-v3-turbo.bin
│   └── ggml-small.bin
├── README.md
├── targets
│   ├── test_1.srt
│   └── test_2.srt
└── test_2.mp4

4 directories, 10 files
```

## 下载模型并保存

```bash
docker run -it --rm \
    -v ./models:/models \
    ghcr.io/ggml-org/whisper.cpp:main \
    "./models/download-ggml-model.sh base /models"
```

## 将音频文件转录为 srt 字幕并保存

### 使用 cpu

```bash
docker run -it --rm \
    -v ./models:/models \
    -v ./audios:/audios \
    -v ./targets:/targets \
    ghcr.io/ggml-org/whisper.cpp:main \
    "whisper-cli -m /models/ggml-base.bin -f /audios/test_1.wav --output-srt -of /targets/test_1"
```

### 使用 gpu

* https://wiki.archlinux.org/title/Docker#Run_GPU_accelerated_Docker_containers_with_NVIDIA_GPUs
* https://github.com/ggml-org/whisper.cpp/issues/2032

```bash
docker run -it --rm --gpus all \
    -e LD_LIBRARY_PATH="" \
    -v ./models:/models \
    -v ./audios:/audios \
    -v ./targets:/targets \
    ghcr.io/ggml-org/whisper.cpp:main-cuda \
    "whisper-cli -m /models/ggml-large-v3.bin -f /audios/test_2.wav -l zh --prompt '遇到中文时，输出简体中文。' --output-srt -of /targets/test_2"
```

## 使用 mpv 播放音频/视频并显示 srt 字幕

```bash
mpv ./audios/test_1.wav --sub-file-paths=../targets --force-window

mpv test_2.mp4 --sub-file-paths=targets
```

## 备注

base 模型处理英文似乎就很不错了。处理中文最好使用更好的模型，例如 `large-v3`
