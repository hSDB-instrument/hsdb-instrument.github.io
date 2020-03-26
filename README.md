## Welcome to hSDB-instrument

Automated surgical tool recognition is a very important fac-tor that can provide feedback on a surgeon’s performance and analyzethem  such  that  it  determines  the  summary  or  training  of  the  surgicalprocedure. Therefore, we introduce a new dataset that reflects the kine-matic characteristics of surgical instruments for automated surgical toolrecognition  of  surgical  videos.  The  hSDB  (human  Surgery  DataBase)-instrument dataset consists of instrument information of cholecystectomyvideos obtained from 24 cases of laparoscopic surgery and gastrectomyvideos obtained from 24 cases of robotic surgery for gastric cancer. Lo-calization information for all tools is provided in the form of a boundingbox for training using the object detection framework. At the same time,to handle class imbalance problem between tools, synthetic tools mod-eled in Unity for three-dimensional (3D) models are included as trainingdata. In addition, for 3D instrument data, a polygon map is provided toenable instance segmentation of the tool. To reflect the kinematic char-acteristics of all tools, they are annotated with head and body parts forlaparoscopic surgery, and with head, wrist, and body parts for roboticsurgery. In addition, annotation data on assistive tools (specimen bag,needle, etc.) that are frequently used for surgery are included. We pro-vide  statistical  information  on  the  hSDB-instrument  dataset  and  thebaseline performance of the object detection networks trained throughthe MMDetection library and the resulting analyses







### Index





### Model Zoo

#### Baselines

More models with different backbones will be added to the model zoo.

| Surgery type | Model         | Bacbone          |    Sub-module     | Epoch | mAP<br />(real) | mAP<br />(real + synthetic) | mAP<br />(real + DR) | mAP<br />(real + syn + DR) | Download |
| ------------ | ------------- | ---------------- | :---------------: | ----- | --------------- | --------------------------- | -------------------- | -------------------------- | -------- |
|              | Cascade R-CNN | HRNetV2p-W18     |         -         |       |                 |                             |                      |                            |          |
|              | Cascade R-CNN | HRNetV2p-W32     |         -         |       | 25.7            | 25.2                        | 25.8                 | 27.1                       |          |
|              | Cascade R-CNN | HRNetV2p-W40     |         -         |       |                 |                             |                      |                            |          |
|              | Cascade R-CNN | ResNext101       |        FPN        |       |                 |                             |                      |                            |          |
|              | Cascade R-CNN | ResNext101-32x4d |        FPN        |       |                 |                             |                      |                            |          |
|              | Cascade R-CNN | ResNext101-64x4d |        FPN        |       |                 |                             |                      |                            |          |
|              | Faster R-CNN  | ResNet50         |        FPN        |       |                 |                             |                      |                            |          |
|              | Faster R-CNN  | ResNet50         |    FPN + OHEM     |       |                 |                             |                      |                            |          |
|              | Faster R-CNN  | ResNet50         | FPN + Double head |       |                 |                             |                      |                            |          |
|              | Faster R-CNN  | ResNet50         |    FPN + Libra    |       |                 |                             |                      |                            |          |
|              | Faster R-CNN  | ResNet101        |        FPN        |       |                 |                             |                      |                            |          |
|              | Faster R-CNN  | ResNext101-32x4d |        FPN        |       |                 |                             |                      |                            |          |
|              | Faster R-CNN  | ResNext101-64x4d |        FPN        |       |                 |                             |                      |                            |          |
|              | Faster R-CNN  | HRNetV2p-W18     |         -         |       |                 |                             |                      |                            |          |
|              | Faster R-CNN  | HRNetV2p-W40     |         -         |       |                 |                             |                      |                            |          |
|              | FCOS          | HRNetV2p-W32-GN  |        FPN        |       |                 |                             |                      |                            |          |
|              | FCOS          | ResNeXt101-64x4d |   FPN + mstrain   |       |                 |                             |                      |                            |          |
|              | FoveaBox      | ResNet50         |        FPN        |       |                 |                             |                      |                            |          |
|              | FoveaBox      | ResNet101        | FPN + align-gn-ms |       |                 |                             |                      |                            |          |
|              | Retinanet     | ResNet50         |    FPN + Libra    |       |                 |                             |                      |                            |          |
|              |               |                  |                   |       |                 |                             |                      |                            |          |
|              |               |                  |                   |       |                 |                             |                      |                            |          |
|              |               |                  |                   |       |                 |                             |                      |                            |          |
|              |               |                  |                   |       |                 |                             |                      |                            |          |









### Getting Started

#### Installation 

##### Requirements

- Linux (Windows is not officially supported)
- Python 3.5+
- PyTorch 1.1 or higher
- CUDA 9.0 or higher
- NCCL 2
- GCC 4.9 or higher
- [mmcv](https://github.com/open-mmlab/mmcv)



##### Install hsdb-instrument mmdetection

1. Create a conda virutal environment and activate it.

```bash
conda create -n open-mmlab python=3.7 -y
conda activate open-mmlab
```

2. Install Pytorch and torchvision follwoing the official instructions, e.g.,

```bash
conda install pytorch torchvision cudatoolkit=10.1 -c pytorch
```

3. Clone the hsdb-instrument detection repository

```bash
git clone https://github.com/hsdb-instrument/hsdb-mmdetection.git
cd hsdb-mmdetection
```

4. Install build requirements and then install hsdb-instrument mmdetection. (We install pycocotools via the github repo instead of pypi because the pypi version is old and not compatible with the latest bumpy.)

```bash
pip install -r requirements/build.txt
pip install "git+https://github.com/cocodataset/cocoapi.git#subdirectory=PythonAPI"
pip install -v -e .  # or "python setup.py develop"
```



Note:

1. The git commit id will be written to the version number with step d, e.g. 0.6.0+2e7045c. The version will also be saved in trained models. It is recommended that you run step d each time you pull some updates from github. If C++/CUDA codes are modified, then this step is compulsory.
2. Following the above instructions, mmdetection is installed on `dev` mode, any local modifications made to the code will take effect without the need to reinstall it (unless you submit some commits and want to update the version number).
3. If you would like to use `opencv-python-headless` instead of `opencv-python`, you can install it before installing MMCV.
4. Some dependencies are optional. Simply running `pip install -v -e .` will only install the minimum runtime requirements. To use optional dependencies like `albumentations` and `imagecorruptions` either install them manually with `pip install -r requirements/optional.txt`or specify desired extras when calling `pip` (e.g. `pip install -v -e .[optional]`). Valid keys for the extras field are: `all`, `tests`, `build`, and `optional`.



#### Inference with pretrained models

We provide testing scripts to evaluate sample images.

You can use the following commands to test sample images.

```bash
./hsdb_demo/demo_inference.sh ${CONFIG} ${CHECKPOINT} ${FRAME_IN_DIR} ${FRAME_OUT_DIR} [--score_thr ${SCORE_THRESHOLD}] [--cholec]
```

Optional arguments:

-  `SCORE_THRESHOLD`: Score threshold value of object detection class probablity. If not specified, the value will be 0.5 as default.
- `--cholec`: If specified, it is assumed that detection model and input images will be of cholecystectomy. If not, gastrectomy.



Examples:

Assume that you have already downloaded the checkpoint and config file to the directory `hsdb_demo/checkpoints_configs/`.

1. 
