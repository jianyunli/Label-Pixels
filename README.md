##  MapChey
MapChey is a tool designed mainly for extracting the road network from the remote sensing imagery and it can be used in other applications applications to label every pixel in the image ( Semantic segmentation). This is part of my MSc research project.
To label every pixel, some list of tools implemented.

#####  Clone repository and install packages
```commandline
git clone https://github.com/venkanna37/RoadMapper.git
conda env create -f environment.yml
```
#####  1. Patch Generation
This generates the patches from the tiles/images and corresponding labels
```commandline
usage: patch_gen.py [-h] [--image_folder IMAGE_FOLDER]
                    [--image_format IMAGE_FORMAT]
                    [--label_folder LABEL_FOLDER]
                    [--label_format LABEL_FORMAT] [--patch_size PATCH_SIZE]
                    [--overlap OVERLAP] [--output_folder OUTPUT_FOLDER]

optional arguments:
  -h, --help            show this help message and exit
  --image_folder IMAGE_FOLDER
                        Folder of input images
  --image_format IMAGE_FORMAT
                        Image format
  --label_folder LABEL_FOLDER
                        Folder of corresponding labels
  --label_format LABEL_FORMAT
                        Label format
  --patch_size PATCH_SIZE
                        Patch size to split the tiles/images
  --overlap OVERLAP     args.overlap between two patches
  --output_folder OUTPUT_FOLDER
                        Output folder to create the images and labels

Example:
python patch_gen.py --image_folder ..\\data\\mass_sample\\test\\image\\ --image_format tiff --label_folder ..\\data\\mass_sample\\test\\label\\ --label_format tif --patch_size 256 --output_folder ..\\data\\mass_patches\\
```

##### 2. CSV Paths
This creates the paths of patches in the csv file
```commandline
usage: csv_paths.py [-h] [--image_folder IMAGE_FOLDER]
                    [--image_format IMAGE_FORMAT]
                    [--label_folder LABEL_FOLDER]
                    [--label_format LABEL_FORMAT] [--pred_folder PRED_FOLDER]
                    [--pred_format PRED_FORMAT] [--output_csv OUTPUT_CSV]

optional arguments:
  -h, --help            show this help message and exit
  --image_folder IMAGE_FOLDER
                        Image folder
  --image_format IMAGE_FORMAT
                        Image format
  --label_folder LABEL_FOLDER
                        Label folder
  --label_format LABEL_FORMAT
                        Label format
  --pred_folder PRED_FOLDER
                        pred_folder
  --pred_format PRED_FORMAT
                        pred_format
  --output_csv OUTPUT_CSV
                        File path of csv that gives image, label paths

Example:
python csv_paths.py --image_folder ..\\data\\mass_patches\\image\\ --image_format tif --label_folder ..\\data\\mass_patches\\label\\ --label_format tif --output_csv ..\\paths\\sample.csv
```

#####  3. Training
Training various networks such as SegNet, Unet and ResUNet
```commandline
usage: train.py [-h] [--model MODEL] [--train_csv TRAIN_CSV]
                [--valid_csv VALID_CSV]
                [--input_shape INPUT_SHAPE [INPUT_SHAPE ...]]
                [--batch_size BATCH_SIZE] [--epochs EPOCHS]
                [--model_name MODEL_NAME]

optional arguments:
  -h, --help            show this help message and exit
  --model MODEL         Model name, should be from unet, resunet, segnet
  --train_csv TRAIN_CSV
                        CSV file with image and label paths from training data
  --valid_csv VALID_CSV
                        CSV file with image and label paths from validation
                        data
  --input_shape INPUT_SHAPE [INPUT_SHAPE ...]
                        Input shape of the model (rows, columns, channels)
  --batch_size BATCH_SIZE
                        Batch size in each epoch
  --epochs EPOCHS       Number of epochs
  --model_name MODEL_NAME
                        Trained model name

Example:
python train.py --model unet --train_csv ..\\paths\\sample.csv --valid_csv ..\\paths\\sample.csv --input_shape 256 256 3 --batch_size 1 --epochs 50 --model_name ..\\trained_models\\sample_256_
```

#####  4. Accuracy
Calculates the accuracy using two metrics such as Iou and F-Scores.
```commandline
usage: accuracy.py [-h] [--model MODEL]
                   [--input_shape INPUT_SHAPE [INPUT_SHAPE ...]]
                   [--weights WEIGHTS] [--csv_paths CSV_PATHS]
                   [--onehot ONEHOT]

optional arguments:
  -h, --help            show this help message and exit
  --model MODEL         Name of the model, should be from unet, resunet,
                        segnet
  --input_shape INPUT_SHAPE [INPUT_SHAPE ...]
                        Input shape of the model (rows, columns, channels)
  --weights WEIGHTS     Name and path of the trained model
  --csv_paths CSV_PATHS
                        CSV file with image and label paths
  --onehot ONEHOT       yes or no, yes if prediction is onehot

Example:
python accuracy.py --model unet --input_shape 256 256 3 --weights ..\\trained_models\\unet_mass_256_300_05_03_20.hdf5 --csv_paths ..\\paths\\sample.csv
```

#####  5. Prediction
Predicthe the entire image/tile using trained model. the image/tile paths can create with `CSV Paths` tool.
```commandline
usage: tile_predict2.py [-h] [--model MODEL]
                        [--input_shape INPUT_SHAPE [INPUT_SHAPE ...]]
                        [--weights WEIGHTS] [--csv_paths CSV_PATHS]
                        [--patch_size PATCH_SIZE] [--tile_size TILE_SIZE]
                        [--output_folder OUTPUT_FOLDER]

optional arguments:
  -h, --help            show this help message and exit
  --model MODEL         Name of the model, should be from unet, resunet,
                        segnet
  --input_shape INPUT_SHAPE [INPUT_SHAPE ...]
                        Input shape of the model (rows, columns, channels)
  --weights WEIGHTS     Name and path of the trained model
  --csv_paths CSV_PATHS
                        CSV file with image and label paths
  --patch_size PATCH_SIZE
                        Patch size of the tile
  --tile_size TILE_SIZE
                        Images size expected (Tile should be in square size
  --output_folder OUTPUT_FOLDER
                        Output path of the predicted images

Example:
python tile_predict2.py --model resunet --input_shape 256 256 3 --weights ..\\trained_models\\resunet_mass_256_300_27_12_19.hdf5 --csv_paths ..\\paths\\sample_tiles.csv --patch_size 256 --tile_size 1500 --output_folder ..\\data\\mass_sample\\test\\pred_resunet\\
```

##### Other tools

##### 1. Summary of the Model
shows the summary of the models
```commandline
usage: summary.py [-h] [--model MODEL]
                  [--input_shape INPUT_SHAPE [INPUT_SHAPE ...]]

optional arguments:
  -h, --help            show this help message and exit
  --model MODEL         Model name, should be from unet, resunet, segnet
  --input_shape INPUT_SHAPE [INPUT_SHAPE ...]
                        Input shape of the model (rows, columns, channels)

Example:
python summary.py --model unet --input_shape 256 256 3
```

#####  2. Rasterize
#####  3. Visualize Kernels
#####  2. Visualize test data