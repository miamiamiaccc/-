# 电商平台智能巡检系统

#### 介绍
{**环境准备**
需要TensorRT、Clip、YOLO的环境配置,最后将pt权重文件下载到本地，yolov8预训练权重是在COCO数据集上进行训练的，若识别效果不佳可自建数据集进行训练

pip install -r requirements.txt

#### 数据集格式要求
（YOLO所需数据集格式为
dataset/
├── images/
│   ├── train/
│   │   ├── image1.jpg
│   │   ├── image2.jpg
│   │   └── ...
│   └── val/
│       ├── image3.jpg
│       ├── image4.jpg
│       └── ...
├── labels/
│   ├── train/
│   │   ├── image1.txt
│   │   ├── image2.txt
│   │   └── ...
│   └── val/
│       ├── image3.txt
│       ├── image4.txt
│       └── ...
├── classes.txt
└── data.yaml）

Clip数据集需要的格式
/dataset
├── images
│   ├── image1.jpg
│   ├── image2.jpg
│   └── ...
├── texts
│   ├── text1.txt
│   ├── text2.txt
│   └── ...
└── index.json

索引文件通常为 JSON 格式，用于描述图像和文本之间的对应关系
[
    {
        "image": "images/image1.jpg",
        "text": "texts/text1.txt"
    },
    {
        "image": "images/image2.jpg",
        "text": "texts/text2.txt"
    }
]


#### 流程简述

利用CLIP多模态模型，实现对电子商城商品页面的自动化巡检，检测商品图片与描述信息的一致性，确保页面内容的合规性。
技术栈：CLIP、YOLOv8、TensorRT
主要工作：1、使用CLIP对商品图片和商品的文本描述进行多模态对齐，通过对比学习判断图片与文本是否匹配
2、结合YOLOv8对商品图片中的关键信息（如商品标签、LOGO）进行检测，进一步提升巡检的准确性
3、使用Pytorch框架对模型进行训练，并用TensorRT进行推理加速，确保系统能处理大量商品页面
4、设计了一套自动化循检流程，定期对电子商城的商品页面进行扫描生成巡检报告并标注异常商品，减少了人力投入

#### 使用说明

环境配置好后，执行核心代码
python ecommerce_inspection_system.py

注意修改ecommerce_inspection_system.py中模型位置后，修改配置文件config.json中相应的模型位置！！！！！

#### 目前功能

1、多模态对齐：
    使用CLIP模型进行图片-文本匹配
    计算相似度得分
    支持自定义文本描述
2、商品检测：
    使用YOLOv8l模型进行目标检测
    检测商品标签、LOGO等关键信息
    支持置信度阈值设置
3、性能优化：
    使用TensorRT进行推理加速
    支持多线程并行处理
    自动降级到CPU模式（当GPU不可用时）
4、自动化巡检：
    支持定时巡检任务
    自动生成Excel格式报告
    异常商品标注


#### 说明

系统会自动：
加载CLIP和YOLOv8模型
处理所有商品图片
进行图片-文本匹配
生成巡检报告
报告内容包括：
图片路径
检测时间
检测框坐标
置信度
类别ID
文本相似度得分
你可以通过修改 config/config.json 来：
调整检测阈值
修改文本描述
配置输出格式
设置定时任务


# 接入采购平台的接口
采购平台接口模块：platform_connector.py
平台配置文件：platform_config.json
集成平台连接器：inspector.py
