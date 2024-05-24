+++
title = 'How to Decode Mp4 Using C++ and Ffmepg'
date = 2024-05-22T16:49:05+08:00
draft = false
tags = ['C++', 'ffmpeg', 'mp4']
+++

# 使用 FFmpeg 库来处理 MP4 编解码

## 初始化

FFmpeg 在你的 C++ 项目中，首先需要初始化 FFmpeg。你可以调用 av_register_all() 来注册 FFmpeg 所有的编解码器和格式。

## 打开输入文件

使用 avformat_open_input() 打开 MP4 文件，获取 AVFormatContext 结构体。这个结构体包含了文件的流信息、编解码器等。

## 查找视频流

遍历 AVFormatContext 中的流，找到视频流的索引。

## 获取视频解码器

使用视频流索引，获取视频流的解码器上下文 AVCodecContext。你可以使用 avcodec_find_decoder() 来查找合适的解码器。

## 打开解码器

使用 avcodec_open2() 打开解码器。

## 读取帧

使用 av_read_frame() 读取视频帧。每一帧都包含在 AVPacket 中。

## 解码帧

使用 avcodec_decode_video2() 解码视频帧。解码后的图像数据将存储在 AVFrame 中。

## 编码帧

如果你需要重新编码帧，可以使用 avcodec_encode_video2() 将解码后的帧重新编码。

## 保存帧

将编码后的帧保存到文件中。你可以使用 av_interleaved_write_frame() 将帧写入输出文件。

## 清理资源

最后，别忘了释放所有分配的内存和关闭文件。

以下是使用 C++， FFmpeg 处理 MP4 编解码示例代码：

```c++
extern "C" {
#include <libavformat/avformat.h>
#include <libavcodec/avcodec.h>
}

int main() {
av_register_all();

    AVFormatContext* formatContext = nullptr;
    if (avformat_open_input(&formatContext, "input.mp4", nullptr, nullptr) != 0) {
        // 处理打开文件失败的情况
        return -1;
    }

    // 查找视频流
    int videoStreamIndex = -1;
    for (unsigned int i = 0; i < formatContext->nb_streams; ++i) {
        if (formatContext->streams[i]->codecpar->codec_type == AVMEDIA_TYPE_VIDEO) {
            videoStreamIndex = i;
            break;
        }
    }

    if (videoStreamIndex == -1) {
        // 没有找到视频流
        return -1;
    }

    // 获取视频解码器
    AVCodec* codec = avcodec_find_decoder(formatContext->streams[videoStreamIndex]->codecpar->codec_id);
    if (!codec) {
        // 没有找到解码器
        return -1;
    }

    AVCodecContext* codecContext = avcodec_alloc_context3(codec);
    if (avcodec_parameters_to_context(codecContext, formatContext->streams[videoStreamIndex]->codecpar) < 0) {
        // 初始化解码器上下文失败
        return -1;
    }

    // 打开解码器
    if (avcodec_open2(codecContext, codec, nullptr) < 0) {
        // 打开解码器失败
        return -1;
    }

    // 读取和处理帧的代码
    AVPacket packet;
    AVFrame* frame = av_frame_alloc();

    while (av_read_frame(formatContext, &packet) >= 0) {
        if (packet.stream_index == videoStreamIndex) {
            if (avcodec_send_packet(codecContext, &packet) == 0) {
                while (avcodec_receive_frame(codecContext, frame) == 0) {
                    // 处理视频帧，例如保存到文件或进行其他操作
                    // 这里只是简单地打印帧的宽度和高度
                    std::cout << "Frame width: " << frame->width << ", height: " << frame->height << std::endl;
                }
            }
        }
        av_packet_unref(&packet);
    }

    av_frame_free(&frame);

    // 清理资源
    avcodec_free_context(&codecContext);
    avformat_close_input(&formatContext);

    return 0;

}
```
