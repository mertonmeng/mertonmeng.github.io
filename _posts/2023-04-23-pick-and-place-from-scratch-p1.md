---
layout: post
title:  "Build Pick-and-Place application from Scratch - Part. 1 Setup"
date:   2023-04-23 12:30:30 -0700
tags: Robotics, CV, DL
---
# Introduction
Have been working in Robotics and CV fields for 10+ years now since college, and it's time to build some cool and fun projects that can summarize all I have learned and teach me what I want to learn. So building a pick-and-place application comes across my mind as it requires all kinds of knowledge across CV and Robotics domain. It is just a perfect project to start with. To 1-up the challenge, here are some constrains I am setting for myself:
- I will avoid to use traditional method as much as possible, everything today is a Machine/Deep Learning based, so it would be lame to stick to some method that has not changed since 20 years ago. For example, I will not use SIFT to do pose estimation.
- Try to build stuff from scratch as much as possible to help me understand problems better. For example, I am not going to use the built-in function in OpenCV to do camera calibration. 

I will start with 2D Pick-and-place and then do 3D pick-and-place

In this blog, I will document the hardware and software setup I have for this project
# Setup
## Hardware Setup
Of course we need hardware setup. And we need the following:
- Robot Arm
- PC with GPU
- Camera/Sensor
### Robot Arm
For the robot arm, I am picking this one:
[Elephant Robotics MyCobot 320 with M5 Stack](https://www.elephantrobotics.com/en/mycobot-320-m5-en/)

Its repeatability is about 1mm, and Max payload is about 1kg. It supports Python and ROS APIs. You can find more specs in their official Website

This thing costs about 3000USD new with an adaptive grabber
### PC
I am using my windows laptop with Nvidia RTX2070 to train the models
### Camera
To achieve 3D pick-and-place more easily. I need some depth sensor. The one I am choosing is [Kinect Azure](https://www.microsoft.com/en-us/d/azure-kinect-dk/8pp5vxmd9nhq?activetab=pivot:overviewtab&culture=en-us&country=us)
## Software Setup
Python is all you need.
### Environment and Package management
[Anaconda](https://www.anaconda.com/) recommended. You can also setup your own pip env.
### Deep Learning Library
[Pytorch](https://pytorch.org/) 1.12 (because my old GPU)
### Kinect Python SDK
https://github.com/ibaiGorordo/pyKinectAzure

This is a third-party python wrapper of the Kinect SDK
### Robot Python SDK
The one comes with the robot. It is called pymycobot, you can find how to setup in their website
### Other Libraries
- OpenCV: When I say I am building from scratch does not mean I want to even to rewrite some color conversion functions. So yes, I still need OpenCV
- AprilTag: This is a good fiducial for camera and robot calibration. I am using: [pupil-apriltags](https://github.com/pupil-labs/apriltags)
- Some Library I may need in the future, and I will document in that post accordingly.