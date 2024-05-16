+++
title = 'The Implement of Qos - 1'
date = 2024-05-16T15:14:18+08:00
draft = false
tags = ['QoS']
+++

# 在 C++中实现 QoS（Quality of Service，服务质量）策略

在 C++中实现 QoS（Quality of Service，服务质量）策略通常涉及到网络通信和数据传输的可靠性、效率和性能。以下是一些常见的 QoS 策略及其在 C++中的应用：

## 历史记录（History）:

保留近期记录（Keep last）：缓存最多 N 条记录，可通过队列长度选项来配置。
保留所有记录（Keep all）：缓存所有记录，但受限于底层中间件可配置的最大资源。

## 深度（Depth）:

队列深度（Size of the queue）：只能与 Keep last 配合使用。

## 可靠性（Reliability）:

尽力的（Best effort）：尝试传输数据但不保证成功传输（当网络不稳定时可能丢失数据）。
可靠的（Reliable）：反复重传以保证数据成功传输。

## 持续性（Durability）:

局部瞬态（Transient local）：发布器为晚连接（late-joining）的订阅器保留数据。
易变态（Volatile）：不保留任何数据。

在 C++中，你可以使用各种库和框架来实现这些 QoS 策略，例如 ROS2 中的 DDS（Data Distribution Service）实现，或者直接使用网络编程库如 Boost.Asio 来控制数据包的发送和接收行为。

如果你正在使用 ROS2，你可以在你的发布器（Publisher）和订阅器（Subscriber）中指定 QoS 配置文件，来确保数据按照你的要求传输。
