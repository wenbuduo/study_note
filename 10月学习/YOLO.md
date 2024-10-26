参考资料：[写给小白的YOLO介绍 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/94986199)
## 背景
识别难题：位置，大小（不能说大象一定比蚂蚁小，因为还有远近之分），种类
最原始的方法是[滑窗法](https://zhida.zhihu.com/search?content_id=109237759&content_type=Article&match_order=1&q=%E6%BB%91%E7%AA%97%E6%B3%95&zhida_source=entity)，就是用[滑动窗口](https://zhida.zhihu.com/search?content_id=109237759&content_type=Article&match_order=1&q=%E6%BB%91%E5%8A%A8%E7%AA%97%E5%8F%A3&zhida_source=entity)去识别一个个物体。我们管滑窗每次滑动的距离叫做**步长**，如果我们把步长设置的特别小，如果步长仅仅为一个像素点，那**一定可以**保证物体可以**正好**出现在某个窗口中了。

## 实现步骤
对于x和y而言，这相对比较容易，毕竟x和y是物体的中心位置，既然物体的中心位置在这个grid之中，那么只要让真实的**x除以grid的宽度**，让真实的**y除以grid的高度**就可以了。

但是h和w就不能这么做了，因为一个物体很可能远大于grid的大小，预测物体的高和宽很可能大于bounding box的高和宽，这样w除以bounding box的宽度，h除以bounding box的高度依旧**不在0和1之间**。

解决方法是让**w除以整张图片的宽度**，**h除以整张图片的高度**。
![[Pasted image 20241026113242.png]]
![[Pasted image 20241026113447.png]]
![[Pasted image 20241026113544.png]]
## 损失函数
![[Pasted image 20241026113602.png]]
![[Pasted image 20241026113828.png]]
## 小破站学习总结
目标检测[4. 4目标检测之性能指标和计算方法._哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1rt4y1W7Dc?spm_id_from=333.788.player.switch&vd_source=f2df16d401e965a4937297d398f6daff&p=4)
[4. 4目标检测之性能指标和计算方法._哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1rt4y1W7Dc?spm_id_from=333.788.player.switch&vd_source=f2df16d401e965a4937297d398f6daff&p=4)![[Pasted image 20241029142534.png]]
![[Pasted image 20241029142600.png]]
![[Pasted image 20241029142623.png]]
![[Pasted image 20241029142648.png]]
![[Pasted image 20241029142809.png]]
**对于PASCALVOC挑战，如果loU>0.5，则预测为正样本(TP)。但是，如果检测到同一目标的多个检测，则视第一个检测为正样本(TP)，而视其余检测为负样本(FP)**
严格一点当吻合框增大到大于0.9时，recall会减小
![[Pasted image 20241029143126.png]]
![[Pasted image 20241029143239.png]]![[Pasted image 20241029143538.png]]