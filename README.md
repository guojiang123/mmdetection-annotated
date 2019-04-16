# mmdetection-annotated
Note on mmdetection for better usage and understand.</br>
## Introduction
Refer to the execllent implemention here:https://github.com/open-mmlab/mmdetection ,and thanks to author [Kai Chen](https://github.com/hellock).</br>
Open-mmlab project , which contains various models and inplementions of latest papers , achieves great efforts in detection/segmentataion tasks , and is kind enough for rookies in CV field.</br>
## Getting started
More information about installation or pre-train model downloads , pls refer to [mmdetection](https://github.com/open-mmlab/mmdetection)</br>
* **Test on images</br>**
You can test on Mask-RCNN demo by running the script `demo.py`
I have just rewritten the demo file to detect on single image or a folder as follow:
```
import ipdb
import sys,os,torch,mmcv
from mmcv.runner import load_checkpoint
#下面这句import的时候定位并调用Registry执行了五个模块的注册
from mmdet.models import build_detector	
from mmdet.apis import inference_detector, show_result

if __name__ == '__main__':
	# ipdb.set_trace()
	cfg = mmcv.Config.fromfile('configs/mask_rcnn_r101_fpn_1x.py')
	# cfg = mmcv.Config.fromfile('configs/faster_rcnn_r50_fpn_1x.py')
	cfg.model.pretrained = None		#inference不设置预训练模型
	#inference只传入cfg的model和test配置，其他的都是训练参数
	model = build_detector(cfg.model, test_cfg=cfg.test_cfg)
	_ = load_checkpoint(model, 'weights/mask_rcnn_r101_fpn_1x_20181129-34ad1961.pth')
	# print(model)  # 展开模型

	# test a single image
	img= mmcv.imread('/py/pic/2.jpg')
	result = inference_detector(model, img, cfg)
	show_result(img, result)

	# # test a list of folder
	# path='/py/mmdetection/images/'
	# imgs= os.listdir(path)
	# for i in range(len(imgs)):
	# 	imgs[i]=os.path.join(path,imgs[i])
	# # imgs = ['/py/pic/4.jpg', '/py/pic/5.jpg']
	# for i, result in enumerate(inference_detector(model, imgs, cfg, device='cuda:0')):
	#     print(i, imgs[i])
	#     show_result(imgs[i], result)

```
* **Debug**</br>
You can debug on code by setting breakpoint with method of add `ipdb.set_trace()`
* **Hook**</br>
If you want to inspect on intermediate variables , `hook.py` can be provided as a reference for your work.
## Annotations</br>
Annotations are attached everywhere in the codes(surely only the part I have read , and the not finished part will be conpleted as soon as possible).Beside , `annotation` folder contains some interpreting documents as well.</br>
* **Model visualization**

