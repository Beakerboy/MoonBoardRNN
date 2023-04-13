# MoonBoardRNN

This repository features 3 key models:

(1) BetaMove, a preprocessing code which converts a MoonBoard problem into a move sequence that is similar to the predicted move sequence of climbing experts.

(2) DeepRouteSet, which was trained using the sequence data generated by BetaMove. Similar to music generation, DeepRouteSet learns the pattern between adjacent moves from existing MoonBoard problems, and is able to generate new problems.

(3) GradeNet, which was also trained using the sequence data generated by BetaMove, and is able to predict the Grade of a MoonBoard problem.

This is the project repository for the CS230 Spring 2020 course project, and is jointly developed by Yi-Shiou Duh and Ray Chang. All our experiments were done in jupyter notebook format.

## Overview of our repository

### `raw_data`
This folder contains the hold difficulty scores evaluated by climbing experts (`\raw_data\HoldFeature2016LeftHand.csv`, `\raw_data\HoldFeature2016RightHand.csv`, `\raw_data\HoldFeature2016.xlsx`) and our scraped raw data from the MoonBoard website (`\raw_data\moonGen_scrape_2016_final`).

The MoonBoard is a 11x18 grid, with holds in a subset of the positions. The hold difficulties are stored in the following files:

- `HoldFeature2016LeftHand` hold difficulties when using left hand.
- `HoldFeature2016RightHand` hold difficulties when using right hand.
- `HoldFeature2016` hold difficulties in an array of 6 values - what do they mean?

The raw data from the MoonBoard website was scraped using the code in https://github.com/gestalt-howard/moonGen. We acknowledged the permission from Howard (Cheng-Hao) Tai to use that code.

### `preprocessing`
There are 4 jupyter notebooks in the `preprocessing` folder:
`Step1_data_preprocessing_v2.ipynb`: separate the problems based on (1)whether the problems is benchmarked or not and (2)whether there is a user grading.
`Step2_BetaMove.ipynb`: This program first computes the success score of each move by the relative distance between holds and the difficulty scale of each hold. It then finds the best route using beam search algorithm.
`Step3_partition_train_test_set_v2.ipynb`: This program divide the dataset into training/dev/test sets.

The final files that are used for the training and evaluation of GradeNet are `training_seq_n_12_rmrp0`, `dev_seq_n_12_rmrp0`, and `test_seq_n_12_rmrp0`.

The final files that are used for the training of DeepRouteSet are `nonbenchmarkNoGrade_handString_seq_X`, `benchmark_handString_seq_X`, `benchmarkNoGrade_handString_seq_X`, and `nonbenchmark_handString_seq_X`.

### `model`
This folder contains 3 files that are critical to run the experiment and repeat our results.

#### GradeNet

To run GradeNet, open the jupyter notebook in `\model\GradeNet.ipynb` and follow the instructions. You can either re-run the experiments, or load the pretrained weights `\model\GradeNet.h5`.

#### DeepRouteSet

To run GradeNet, open the jupyter notebook in `\model\DeepRouteSet_v4.ipynb` and follow the instructions. You can either re-run the experiments, or load the pretrained weights `\model\DeepRouteSetMedium_v1.h5`. The code in this file is largely modified from a Coursera problem exercise "Improvise a Jazz Solo with an LSTM Network", which is originally modified from https://github.com/jisungk/deepjazz.

#### Predict the grade of generated problems

To evaluate the generated problems, open the jupyter notebook in `\model\Evaluate_Generated_Output_v3.ipynb` and follow the instructions. Please remember to check if `raw_path` is correct.


### `out`
This folder contains our generated problems. The folder `\out\DeepRouteSet_v1` contains style prediction results of our generated data. Those style predictions are very preliminary, and please ignore that part.

### `website`
This is a static website that shows the 65 generated MoonBoard problems using DeepRouteSet. The link to the website: https://jrchang612.github.io/MoonBoardRNN/website/.

The layout of this website is modified from https://github.com/andrew-houghton/moon-board-climbing and is granted permission from the authors.

## Potential future items
* StyleNet


## New notes from BouldAR

general advice:

- when running Jupyter Notebooks remotely, be aware that many images in the output will result in very slow performance due to synchronisation with the server.
- the `pyproject.toml` will need to be adjusted for different tensorflow environments e.g. CUDA, MAC

### Project structure:
├───`model` generation and grading model training & prediction
│   │   `DeepRouteSetHelper.py`                   helper functions for DeepRouteSet
│   │   `DeepRouteSetMedium_v1.h5`                model weights for deep route set TODO: whats medium?
│   │   `DeepRouteSet_v4.ipynb`                   DeepRouteSet model training and prediction
│   │   `Evaluate_Generated_Output_v3.ipynb`      Evaluation of GradeNet prediction of generated problems
│   │   `GeneratedRoutes`                         
│   │   `generator.py`                            script to generate problems of a given grade
│   │   `GradeNet.h5`                             GradeNet model weights
│   │   `GradeNet.ipynb`                          GradeNet model training and prediction
│   │   `GradeNet_train_history`                  
│   │   `MediumProblemOfDeepRouteSet_v1`          
│   │   `MediumProblemSequenceOfDeepRouteSet_v1`  
│   │   `model_helper.py`                         helper functions for GradeNet 
│   │
│   ├───`DeepRouteSet` DeepRouteSet saved model
│   │
│   ├───`logs` training logs
│
├───`out`
│   │   `GeneratedRoutes`
│   │   `MediumProblemOfDeepRouteSet_v1`
│   │   `MediumProblemSequenceOfDeepRouteSet_v1`
│   │   `ProblemOfDeepRouteSet_v1`
│   │   `ProblemSequenceOfDeepRouteSet_v1`
│   │
│   ├───`DeepRouteSet_v1`
│   │       `DeepRouteSet_v1_idX.jpg`
│   │       `DeepRouteSet_v1_idX_predicted_style.jpg`
│   │
│   └───`GeneratedRoutes_v0`
│           `genX.jpg`
│
├───`preprocessing` preprocessing of raw data
│       `benchmarkNoGrade_handString_seq_X`       
│       `benchmark_handString_seq_X`              
│       `benchmark_nograde_move_seq_X`            
│       `benchmark_nograde_move_seq_Y`            
│       `benchmark_withgrade_move_seq_X`          
│       `benchmark_withgrade_move_seq_Y`          
│       `constants.py`                            constants used in preprocessing
│       `dev_seq_n_12_rmrp0`                      
│       `nonbenchmarkNoGrade_handString_seq_X`    
│       `nonbenchmark_handString_seq_X`           
│       `nonbenchmark_nograde_move_seq_X`         
│       `nonbenchmark_nograde_move_seq_Y`         
│       `nonbenchmark_withgrade_move_seq_X`       
│       `nonbenchmark_withgrade_move_seq_Y`       
│       `preprocessing_helper.py`                 helper functions for preprocessing
│       `processed_data_seq`                      
│       `processed_data_xy_mode`                  
│       `Step1_data_preprocessing_v2.ipynb`       step 1: aasdsd
│       `Step2_BetaMove.ipynb`                    step 2: asda
│       `Step3_partition_train_test_set_v2.ipynb` step 3: askdkasodk
│       `test_seq_n_12_rmrp0`                     
│       `test_set_medium_gen_v1`                  
│       `training_seq_n_12_rmrp0`                 
│       `X_seq_dict_merge`                        
│       `Y_seq_dict_merge`                        
│
├───`raw_data` raw data from moonboard
│       `HoldFeature2016.csv`
│       `HoldFeature2016LeftHand.csv`
│       `HoldFeature2016RightHand.csv`
│       `holdIx_to_holdStr`
│       `holdStr_to_holdIx`
│       `moonboard2016Background.jpg`     MoonBoard 2016 board image
│       `MoonBoard_2016_raw.json`
│       `MoonBoard_2016_raw_schema.json`
│       `moonGen_scrape_2016`
│       `moonGen_scrape_2016_cp`
│       `moonGen_scrape_2016_fail`
│       `moonGen_scrape_2016_final`
│