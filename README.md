# Graph Convolutional Transformer replicate

This repository contains code for running a Graph Convolutional Transformer. Below are the steps to run the code:

## Download the data to your environment
- The eICU data comes from https://physionet.org/content/eicu-crd/2.0/.
- You are required to participate in the CITI training.
- Download the patient, admissionDx, diagnosis, treatment CSV files
- Decompress and upload to your environment.
  
## Cloud:
  
### Copy the DL4H_Team_137.ipyhnb in the google colab next to the data download
- adjust the `raw_data_dir` according to your url shown

## Local:

### To run the original paper code at https://github.com/Alexuiuc/paperReplicateForDHL

### Setup Instructions:

1. **Clone Repository:** 
    - Fetch the repository into your local environment.

2. **Navigate to Directory:** 
    - Go to the directory of `graph-convolutional-transformer/`.

3. **Compatibility Fixes:** 
    - Due to compatibility issues, make the following adjustments:
        - Remove the `import sklearn ...` statements from the two `.py` files.
        - Change function calls such as `ms.train_test_split` to `train_test_split`.
        - Add the following function at the top of the two `.py` files:
            ```python
            import random
            
            def train_test_split(data, test_size=0.2, random_state=None):
                if random_state is not None:
                    random.seed(random_state)
                
                data_shuffled = data[:]
                random.shuffle(data_shuffled)
                
                split_idx = int(len(data) * (1 - test_size))
                
                train = data_shuffled[:split_idx]
                test = data_shuffled[split_idx:]
                
                return train, test
            ```

4. **Environment Setup:** 
    - Install `conda` if not already installed.
    - Create a new environment named `DHLFinalProject` with Python version 2.7:
        ```bash
        conda create -n DHLFinalProject python=2.7
        ```
    - Activate the environment:
        ```bash
        conda activate DHLFinalProject
        ```
    - Install TensorFlow version 1.13.1:
        ```bash
        conda install tensorflow==1.13.1
        ```

5. **Data Preparation:** 
    - Create a directory named `output`.
    - Copy data downloaded from Citi to the `graph-convolutional-transformer/` directory.

6. **Processing eICU Data:** 
    - Run the following command in the terminal to process the eICU data:
        ```bash
        python eicu_samples/process_eicu.py . ./output
        ```

7. **Adjust File Paths:** 
    - Remove the `.` in the `train.py` and `validation.py` files for file paths like `./train.tfrecord` and `./validation.tfrecord`.

8. **Create Result Directory:** 
    - Create a directory named `trainResult/fold_0/` in the project directory.

### Training:

9. **Training Execution:** 
    - Set `num_iter = 4800` in the `train.py` file.
    - Run the following command in the terminal:
        ```bash
        python train.py output/fold_0 trainResult/fold_0
        ```

10. **Monitoring Progress:** 
    - The training process will take approximately 2 hours.
    - Upon completion, you will receive results in the terminal similar to:
        ```
        INFO:tensorflow:Evaluation [10/100] ...
        INFO:tensorflow:Evaluation [20/100] ...
        ...
        INFO:tensorflow:Finished evaluation at 2024-04-13-09:19:03
        INFO:tensorflow:Saving dict for global step 4800: AUC-PR = 0.31366622, AUC-ROC = 0.676465, global_step = 4800, loss = 0.5836665.
        ```

### Notes:

- **Demo Result:**
    - The demo train result in one fold is: 
        - AUC-PR = 0.31366622
        - AUC-ROC = 0.676465
        - Global Step = 4800
        - Loss = 0.5836665.


