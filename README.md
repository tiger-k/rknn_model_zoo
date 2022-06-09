<!--- SPDX-License-Identifier: Apache-2.0 -->
## 调试记录
1.环境配置
  ubuntu18.4  python 3.6  板子rk3399proD
  pip安装依赖版本 参照 my_requirement.txt

2.模型训练
  python train.py --weights yolov5s.pt --cfg models/yolov5s.yaml--data data/test.yaml --epochs 1
  此处 yolov5s.pt 从V4.0下载 
  url: https://github.com/ultralytics/yolov5/releases/download/v4.0/yolov5s.pt
  data中只有一张图片，可以快速训练验证（只能验证流程能否通过，不能识别目标）
3. pt转换onnx
  python models/export.py --rknn_mode --weights runs/train/exp/weights/best.pt
  此处 --weights后面的参数是刚刚训练生成的.pt文件
  在runs/train/exp/weights/下生成best.onnx

4. onnx转rknn
  python rknn_convert.py 
  此处需要修改 convert/config.yaml
  running:
    model_type: onnx
    export: True
    inference: false
    eval_perf: True
  onnx:
    model: '../runs/train/exp2/weights/best.onnx'  #修改为上步导出的onnx的路径 搞不清的话就写绝对路径
  rknn:
    target_platform: ['rk3399pro']
    dataset: './dataset.txt'
    export_path: './best.rknn' #生成rknn
    target: rk3399pro
5.上板运行
  python3 rknn_detect_for_yolov5_original.py
  
目前只做到这一步，关于C++推理
