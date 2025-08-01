---
title: Design Goals
author: Simone Massaro
date: 24 July 2025
---

# Introduction

Larix aims to be the "laser infrastructure" library, which provides modern foundations for point cloud processing.
In the last decade there are been significant advancements in data processing libraries that increase the performance and usability of data processing.
For data frames we have Polars, which is an excellent example of what we can achieve with a library design from the ground up and effectively utilize modern hardware.
At the same time the point cloud library ecosystem is fragmented and with often design limitations.
This is a ridiculously ambitious goal, the hope that it will at least start a conversation of the point cloud library of the future and building proof of concepts.

## Why Point Clouds


We are at inflection point for point clouds, LiDAR sensors are becoming much cheaper and available at the importance of point clouds has been proven in many different field from robotics to forestry.
At the same time the software for processing point cloud is lagging behind, compared to 2D images is decades behind.
While it would be naive to think that we can have point clouds can have the same level of support of 2D data (e.g. custom silicon) we can do a lot better than what we have today.
In the last decade the power of GPUs has exploded, and they are not only useful for doing matrix multiplications but also well suited for point cloud processing.

Point clouds often contains a lot of data, which makes them useful for many applications, but also results in large files that are difficult to process efficiently.

I think is possible to have better software for point cloud processing and we should invest on it!
Efficient point cloud processing can unlock


# Design goals

1. Fast, achieve state-of-the art performance
2. Scalable, transparently scale from a single core, to large than memory dataset to an entire cluster
3. Modern hardware, optimized for GPUs and modern CPUs
4. Multi-level API, expose API from all levels of abstractions
5. All LiDAR sensors, from robotic LiDAR to full-waveform satellite LiDAR
6. Visualization, effective and efficient point cloud visualization
7. Machine Learning, easy interoperability with machine learning frameworks
8. Robust, well tested and proper error handling

## Why

### Fast

The library needs to achieve state-of-the-art performance. If it doesn't users will end up using other libraries that are faster,
which totally defeats the ideas of larix as the foundation layer.
An example of a library that doesn't achieve this goal is PDAL, that while it provides a large set of features is often pretty slow, which limits its usability.

## Scalable

Point clouds processing happens at different scales, from a developer's laptop to a large cluster. Point Clouds can get very big very quickly, making the use powerful hardware necessary.
At the same time the ability to scale should not impact the use on a single machine, which remains a key use case.
A major source of fraction with the current libraries is scaling to large scale processing, which requires custom solutions.
We take inspiration from Polars, that with the same API it handles datasets processing from a laptop to the cloud.
Scalability also means that the file format need to be cloud native, allowing efficient querying of a subset of the data, and support larger than memory datasets.

## Modern hardware

We have GPUs, while everyone is using them for AI they are also perfect of point cloud processing! Moreover modern CPUs are much more than a super-fast PDP-11, need to effectively utilize their capabilities.
To achieve the best performance we need to make the best use of the features of the hardware.

## Multi-Level API

As a foundation library we need to provide access to all APIs, so that users can implement their own algorithms and add features add the level that they need.
First class support for a Python API is must.
This also means the there is the need for proper documentation at all levels of API.

## All LiDAR sensors

LiDAR is used in a wide range of applications, from autonomous vehicles to environmental monitoring. There is a split between different communities:
the robotics community that develops a lot of low-level algorithms and while the geospatial community that has systems for large scale point cloud processing but often with a limited feature set.
This leaves terrestrial LiDAR in an awkward middle ground.
Finally, no assumption should be made about the type of sensor [^1]

[^1]: For example PDAL LAS writer defaults to a scale of 0.01 meters, which is good for aerial LiDAR but definitely not for terrestrial LiDAR.

## Visualization

A limiting factor for the use of LiDAR data is that it is hard to visualize it, we should remember that interactions with humans are critical to understand and use the data.

## Machine Learning

A lot of recent innovation in point cloud processing (e.g. segmentation) utilize deep learning models. A future looking point cloud library needs to have first class integration with deep learning frameworks.

## Robust

The foundation layer needs to be rock solid, this means it should be well tested so we can expect its stability and reliability.
Moreover, when error occurs it should strive to provide meaningful and actionable error messages.

# How

## Fast

To build a fast library we need to think about performance from the beginning. This means creating a representative benchmark suite and use it guide the development of the library and catch performance regressions.

## Modern Hardware

- SIMD. We need SIMD otherwise we are leaving a lot of performance on the table. This means we can't leave SIMD to the goodness of the compiler, but nee to explicity and consistently use SIMD.
- know the CPU, then to remember the there is a cache, a branch predictor and that a CPU executes multiple instructions per cycle.
- Multi-cores
- GPUs

## Robust

We need to use a memory-safe and thread-safe programming language.
Need to have a robust test suite and probably also have a fuzzer.

## Data Structures

### In Memory

- columnar format. This is crucial for good performance especially when using SIMD
- multi-threaded. Need to be safe and fast when using multiple threads (for example trying to avoid locking)

### On disk

# Why a new library?

We need solid foundations to build the future on, here I am exploring why we can't really improve on existing libraries but need to start from scratch.

## PCL

## Open3D

## PDAL

# Isn't this crazy?

Yes, definitely!
However, I think it's worth it and the only way to go if we truly want to have robust foundations.
I have been inspired by other projects that took the hard road of doing the foundations right and then manage to deliver outstanding results. Examples are Zed, that built an entire editor from the ground up, or typst, that is incredible steps forward compared to Latex.
