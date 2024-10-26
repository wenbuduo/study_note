学习视频（【【Python_YOLO简易教程】用20分钟入门YOLOv5目标检测】 https://www.bilibili.com/video/BV1GA411P7vE/?p=2&share_source=copy_web&vd_source=fe59d667f9424d841d598d939d8c404a）
## 调参
命令行（cmd）
```
python detect.py --source 0#webcam
						file.jpg #image 图片
						file.mp4 #视频
						path/#文件夹
						'http://'#链接
						'rtsp://'#手机摄像头之类的实时监测（RTSP,RTMP,HTTP）
```
训练自己的数据集工具 ——labelimg
使用方法：
```
conda activate yolo
pip install labelimng
labelimg
```
标注好数据集之后和代码文件放在同一个根目录下面

![[Pasted image 20241129104601.png]]
现有的数据集网站：
[Computer Vision Datasets (roboflow.com)](https://public.roboflow.com/)