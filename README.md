# BuildingBlock
[**[SIGGRAPH 2025]BuildingBlock: A Hybrid Approach for Structured Building Generation**](https://arxiv.org/pdf/2505.04051) <br>
comming soon... <br>

![pipeline_cropped_00](https://github.com/user-attachments/assets/1fa83ce6-4152-4277-811f-79849562cabc)

![1749536947556](https://github.com/user-attachments/assets/2fbe3d2c-89cf-4583-9e95-04eb447886a1)


## Installation

### 1. Download Docker

Ensure Docker is properly installed on your system. Visit [Docker's official website](https://www.docker.com/get-started) for installation instructions specific to your operating system.

```bash
docker pull dreaminghuang/building_block:0.1
```
### 2. Run Docker Container

Mount your local directory to the container and start an interactive session:

```bash
docker run -it --gpus all -v your_path_of_building_block:/building_block -w /building_block building_block:0.1 /bin/bash
```

Replace `your_path_of_building_block` with the absolute path to your local directory where you want to store the project files.

### 3. Initialize Environment

Run the initialization script to set up the required dependencies:

```bash
bash initialization.sh
```

## Dataset Preparation

### 1. Extract Dataset

Extract the `BoxCenterSizeLabel_all` directory from the archive file:
```bash
unzip building_block_data_opensource_only_layout_and_cond.zip
```

### 2. Process Dataset

Run the following Python scripts in sequence to preprocess the data:

```bash
python 1-json_rotate_augment.py
```
This script performs data augmentation through rotation of the original JSON files.

```bash
python 2-normUeJson.py
```
This script normalizes the Unreal Engine JSON format data.

```bash
python 3-json2boxnp.py
```
This script converts the JSON data to NumPy arrays in box representation format.

```bash
cp dataset_stats.txt ./BoxCenterSizeLabelNp
```

## Running the Model

Navigate to the scripts directory:

```bash
cd scripts
```

Execute the commands provided in `command.sh` to train and/or evaluate the model:

```bash
# View the available commands
cat command.sh

# Execute specific commands as needed
# For example:
python train_diffusion_building_DDP.py ../config/text/diffusion_building_DIT.yaml uncond  --experiment_tag uncond --n_processes 0 --with_swanlab_logger
```

## Project Structure

- `1-json_rotate_augment.py`: Data augmentation script
- `2-normUeJson.py`: JSON normalization script
- `3-json2boxnp.py`: JSON to NumPy conversion script
- `scripts/`: Contains model training and evaluation scripts
- `configs/`: Configuration files for different model settings
- `BoxCenterSizeLabel_all/`: Directory containing the dataset

## Notes

- Make sure your GPU drivers are properly configured for Docker GPU passthrough
- The dataset processing may take significant time depending on the size of the dataset
- Check the log files for any errors during processing

For more detailed information about the model architecture and training parameters, please refer to the documentation in the respective script files.
