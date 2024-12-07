# CSC2529

## Testing other tracking algorithms
The script `object_tracking.py` was used to test the following tracking algorithms from the legacy opencv2 tracker library: 
- "csrt"
- "kcf"
- "boosting"
- "mil"
- "tld"
- "medianflow"
- "mosse"

Example command:
```
python object_tracking.py --video /home/joseph/Documents/project/videos/foosball_videos/Ball\ color\ videos/pink_VID_20220408_065401892.mp4  --tracker "kcf"
```

## Training Object Detector
### Data labelling
`foosball_training_set_labeller.ipynb` uses sam2 to generate masks on objects across the training video.  These masks are then used to generate bounding boxes and label files for a training data set.

This notebook was originally created in my clone of the sam2 repo here: [sam2-copy](https://github.com/jclundy/sam2-copy)

### Training yolov7 model
See the following files `foosball_train.ipynb`

This notebook was originally created in my clone of the sam2 repo here: [yolov7-copy](https://github.com/jclundy/yolov7-copy).  Training parameters and meta data can be viewed in that repo as well.


#### run training
```
python train.py --img-size 640 --cfg cfg/training/yolov7-tiny.yaml --hyp data/hyp.scratch.tiny.foosball.yaml --batch 8 --epochs 100 --data data/foosball_data.yaml --weights yolov7-tiny.pt --workers 24 --name yolo_foosball_det_1
```

#### run detector
```
python detect.py --weights /home/joseph/Documents/git/yolov7/runs/train/yolo_foosball_det_1/weights/best.pt --source /home/joseph/Documents/project/videos/foosball_videos/20221113_121152.mp4
```

#### run tester
```
python test.py --weights runs/train/yolo_foosball_det_1/weights/best.pt --img-size 640 --conf 0.001 --iou 0.64 --data  Datasets/all_ball_colors/data.yaml --task test
```

## Angle estimation
### Ground truth angle estimation
Relevant files:
- camera_calibration_notebook.ipynb
- ground_truth_rotations_from_aruco.ipynb

`camera_calibration_notebook.ipynb` was used to perform a camera calibration for the 2 different cameras used in generating the angle estimation data sets.  

`ground_truth_rotations_from_aruco.ipynb` looks at the video of the aruco markers placed on the rods, as well as calibration images of each foosman aligned vertically, and generates the ground truth angle data, with some filtering. 

### Angle estimation from bounding boxes
`foosball_angle_estimation.ipynb` uses sam2 to generate bounding boxes on the input video, then runs the angle estimation algorithm on the bounding boxes.  It loads the pre-computed 'ground-truth' angle data computed in `ground_truth_rotations_from_aruco.ipynb` for the regression and error analysis.

This notebook was originally created in my clone of the sam2 repo here: [sam2-copy](https://github.com/jclundy/sam2-copy)