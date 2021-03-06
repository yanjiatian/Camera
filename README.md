# Android Camera Demos

**本项目主要有以下几点内容：**

**一、围绕Android相机的各种Demo，包括预览，RGB转换，视频录制和播放，滤镜等，每种Demo都会给出若干种解决方案**

**二、围绕Android相机及图像音频架构和原理做深入分析**

------

## **一、相机预览**

|序号|项目名称|内容简介|状态|
|--- |-------|-------|------|
|1|GLSurfacePreview|GLSurfaceView + OpenGL相机预览，直接绘制到Display Surface|Done|
|2|GLSurfacePreview2|GLSurfaceView + OpenGL相机预览，先绘制到FBO的Texture上，再处理后(变红)绘制到Display Surface|Done|
|3|SurfacePreview|SurfaceView + OpenGL + EGL相机预览，直接绘制到Display Surface|Done|
|4|SurfacePreview2|SurfaceView + OpenGL + EGL相机预览，先绘制到PBuffer，再Blit到Display Surface|Done|
|5|MultiSurfacePreview|相机预览到两个SurfaceView，共享EglContext，先绘制到Texture，再将Texture处理后Draw到另一个Surface|Done|

## **二、RGB转换**
利用GPU将相机帧(NV21)转成RGB并传至CPU，帧为1920 * 1080，RGBA

另开一个线程做RGB转换，不然如果和相机共用上下文，渲染时需要来回切换，且可能阻塞相机渲染，对性能不利。

|序号|模块名称|内容简介|状态|
|--- |-------|-------|-----|
|1|RgbConverter1|从Display Surface直接readPixels，性能很差，~550ms|done|
|2|RgbConverter2|从Pbuffer调readPixels，性能有较大提升，~30ms|done|
|3|RgbConverter3|从FBO调readPixels，性能比PBuffer稍好一点，~27ms|done|
|4|RgbConverter4|从FBO读到PBO，readPixels阻塞, glMapBuffer阻塞，~11ms|done|
|5|RgbConverter5|从Pbuffer读到PBO，readPixels异步, glMapBuffer阻塞，~6ms|done|

这里方式4和5的结果差别的原因暂时没搞清楚，方式5是从Pbuffer的默认FBO读到PBO，方式4是另开的一个FBO读到PBO，这两种应该没太大区别，而结果表明方式5比较理想，glReadPixels应该是异步，阻塞只在glMapBuffer。

## **三，视频录制**

|序号|项目名称|内容简介|状态|
|--- |-------|-------|----|
|1|recorder1|GLSurfaceView + MediaMuxer，不共享EglContext，只能录制相机预览|Pending|
|2|recorder2|SurfaceView + MediaMuxer，共享EglContext，可以录制整个Surface|Pending|
|3|recorder3|只录制Surface上某一个部分，如人脸|Pending|


## **四，视频播放**

|序号|项目名称|内容简介|状态|
|--- |-------|-------|----|
|1|video1|SurfaceView播放原始视频|Pending|
|2|video2|视频裁剪播放，并增加一层遮罩|Pending|


------
有问题或建议可以给我邮件

Email: dingjikerbo@gmail.com