---
layout: post
title: "iOS视频直播开发note1"
author: peiwei
modified:
excerpt: "How to get video stream from camera in iOS"
tags: [iOS,Swift,tutorial,video,AVFoundation]
---

本文章旨在配置摄像头，切换摄像头，获取视频流资源

```

import UIKit
import AVFoundation

class ViewController: UIViewController,AVCaptureVideoDataOutputSampleBufferDelegate {
    lazy var session = AVCaptureSession();
    var videoQueue:dispatch_queue_t!
    var videoOutput:AVCaptureVideoDataOutput!
    var videoConnection:AVCaptureConnection!
    var previewLayer:AVCaptureVideoPreviewLayer!
    
    override func viewDidLoad() {
        super.viewDidLoad()
        self.setupPreviewLayer()
        self.setupSession()

    }
```    

``` 
    func setupSession() {
        // 获得一个采集设备，例如前置/后置摄像头
        let videoDevice = self.cameraWithPosition(AVCaptureDevicePosition.Front);
        if videoDevice == nil {
            return
        }
        // 用设备初始化一个采集的输入对象
        let videoInput:AVCaptureDeviceInput
        do {
            videoInput = try AVCaptureDeviceInput(device: videoDevice)
        } catch {
            return;
        }
        
        if session.canAddInput(videoInput) {
            session.addInput(videoInput)
        }
        
        // 配置采集输出，即我们取得视频图像的接口
        videoQueue = dispatch_queue_create("VideoCaptureQueue", DISPATCH_QUEUE_SERIAL);
        videoOutput = AVCaptureVideoDataOutput()
        videoOutput.setSampleBufferDelegate(self, queue: videoQueue);
        
        // 配置输出视频图像格式  kCVPixelFormatType_420YpCbCr8BiPlanarVideoRange, kCVPixelFormatType_420YpCbCr8BiPlanarFullRange and kCVPixelFormatType_32BGRA.
        videoOutput.videoSettings = [kCVPixelBufferPixelFormatTypeKey: NSNumber(unsignedInt: kCVPixelFormatType_32BGRA)];
        videoOutput.alwaysDiscardsLateVideoFrames = true;
        if session.canAddOutput(videoOutput) {
            session .addOutput(videoOutput)
        }
        
        // 保存Connection，用于在SampleBufferDelegate中判断数据来源（是Video/Audio？）
        videoConnection = videoOutput.connectionWithMediaType(AVMediaTypeVideo)
        
        session.startRunning()
    }
```
```
    func setupPreviewLayer() {
        self.previewLayer = AVCaptureVideoPreviewLayer(session: self.session);
        self.previewLayer.videoGravity = AVLayerVideoGravityResizeAspectFill;
        self.previewLayer.frame = self.view.layer.bounds;
        self.view.layer.addSublayer(self.previewLayer);
        
        let button = UIButton(frame: CGRect(origin: CGPointMake(10, 20), size: CGSizeMake(100, 20)))
        button.setTitle("switch", forState: UIControlState.Normal)
        self.view .addSubview(button);
        button .addTarget(self, action: "switchCameraPosition", forControlEvents: .TouchUpInside)
    }
    
```

```
    func cameraWithPosition(position:AVCaptureDevicePosition!)->AVCaptureDevice? {
        let devices = AVCaptureDevice.devicesWithMediaType(AVMediaTypeVideo);
        for device in devices {
            if device.position == position {
                return device as? AVCaptureDevice;
            }
        }
        return nil;
    }
    
```
 
```
    //切换摄像头  前／后
    func switchCameraPosition() {
        session.beginConfiguration()
        let currentCameraInput = session.inputs[0] as! AVCaptureInput
        session .removeInput(currentCameraInput)
        
        var newCamera:AVCaptureDevice?
        if (currentCameraInput as! AVCaptureDeviceInput).device.position == AVCaptureDevicePosition.Back {
            newCamera = self.cameraWithPosition(AVCaptureDevicePosition.Front)
        } else {
            newCamera = self.cameraWithPosition(AVCaptureDevicePosition.Back)
        }
        if newCamera == nil {
            return;
        }
        
        do {
            let newDeviceInput = try AVCaptureDeviceInput(device: newCamera)
            self.session .addInput(newDeviceInput)
        }catch {
            return;
        }
        self.session.commitConfiguration()
    }

    
```

```
    func captureOutput(captureOutput: AVCaptureOutput!, didOutputSampleBuffer sampleBuffer: CMSampleBuffer!, fromConnection connection: AVCaptureConnection!) {
        if connection == videoConnection {
            print(sampleBuffer);
        }
    }
 }

```


