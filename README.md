# MoonBoardRNN

This repository features 3 key models:

(1) BetaMove, a preprocessing code which converts a MoonBoard problem into a move sequence that is similar to the predicted move sequence of climbing experts.

(2) DeepRouteSet, which was trained using the sequence data generated by BetaMove. Similar to music generation, DeepRouteSet learns the pattern between adjacent moves from existing MoonBoard problems, and is able to generate new problems.

(3) GradeNet, which was also trained using the sequence data generated by BetaMove, and is able to predict the Grade of a MoonBoard problem.

This is the project repository for the CS230 Spring 2020 course project, and is jointly developed by Yi-Shiou Duh and Ray Chang. All our experiments were done in jupyter notebook format.

## Overview of our repository

### `raw_data`
This folder contains the hold difficulty scores evaluated by climbing experts (`\raw_data\hold_features_2016_LH.csv`, `\raw_data\hold_features_2016_RH.csv`, `\raw_data\hold_features_2016.xlsx`) and our scraped raw data from the MoonBoard website (`\raw_data\moonGen_scrape_2016_final`).

The MoonBoard is a 11x18 grid, with holds in a subset of the positions. The hold difficulties are stored in the following files:

- `hold_features_2016_LH` hold difficulties when using left hand.
- `hold_features_2016_RH` hold difficulties when using right hand.
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

To run GradeNet, open the jupyter notebook in `\model\DeepRouteSet_v4.ipynb` and follow the instructions. You can either re-run the experiments, or load the pretrained weights `\model\DeepRouteSet_medium.h5`. The code in this file is largely modified from a Coursera problem exercise "Improvise a Jazz Solo with an LSTM Network", which is originally modified from https://github.com/jisungk/deepjazz.

#### Predict the grade of generated problems

To evaluate the generated problems, open the jupyter notebook in `\model\generated_route_eval.ipynb` and follow the instructions. Please remember to check if `raw_path` is correct.


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
    │   │   `DeepRouteSet_helper.py`                  helper functions for DeepRouteSet
    │   │   `DeepRouteSet_medium.h5`                  model weights for deep route set
    │   │   `DeepRouteSet_medium_out`
    │   │   `DeepRouteSet_medium_out_seq`
    │   │   `DeepRouteSet_train.ipynb`                DeepRouteSet model training
    │   │   `DeepRouteSet_eval.ipynb`                 DeepRouteSet model prediction
    │   │   `generated_route_eval.ipynb`              Evaluation of GradeNet prediction of generated problems
    │   │   `generator.py`                            script to generate problems of a given grade
    │   │   `GradeNet.h5`                             GradeNet model weights
    │   │   `GradeNet_train.ipynb`                    GradeNet model training
    │   │   `GradeNet_eval.ipynb`                     GradeNet model prediction
    │   │   `GradeNet_train_history`
    │   │   `model_helper.py`                         helper functions for GradeNet
    │   │
    │   ├───`DeepRouteSet` DeepRouteSet saved model
    │   │
    │   ├───`logs` training logs
    │   │
    │   ├───`checkpoints` training weight checkpoints
    │
    ├───`out`
    │   │   `MediumProblemOfDeepRouteSet_v1`
    │   │   `MediumProblemSequenceOfDeepRouteSet_v1`
    │   │   `ProblemOfDeepRouteSet_v1`
    │   │   `ProblemSequenceOfDeepRouteSet_v1`
    │   │
    │   ├───`DeepRouteSet_v1`
    │   │       `DeepRouteSet_v1_idX.jpg`
    │   │       `DeepRouteSet_v1_idX_predicted_style.jpg`
    │
    ├───`preprocessing` preprocessing of raw data
    │       `benchmarkNoGrade_handString_seq_X`
    │       `benchmark_handString_seq_X`
    │       `benchmark_nograde_move_seq_X`
    │       `benchmark_nograde_move_seq_Y`
    │       `benchmark_withgrade_move_seq_X`
    │       `benchmark_withgrade_move_seq_Y`

    │       `nonbenchmarkNoGrade_handString_seq_X`
    │       `nonbenchmark_handString_seq_X`
    │       `nonbenchmark_nograde_move_seq_X`
    │       `nonbenchmark_nograde_move_seq_Y`
    │       `nonbenchmark_withgrade_move_seq_X`
    │       `nonbenchmark_withgrade_move_seq_Y`

    │       `processed_data_seq`
    │       `processed_data_xy_mode`

    │       `test_set_medium_gen_v1`
    │       `X_seq_dict_merge`
    │       `Y_seq_dict_merge`

Train/val/test datasets for GradeNet. Each example is a list of moves where each move is described by a 22-dim feature vector.
The Y truths are the grades (adjusted so V4 is 0) of the problems (given as a single float, not one-hot encoded).

    │       `training_seq_n_12_rmrp0`                 training set for GradeNet ({'X': , 'Y': })
    │       `dev_seq_n_12_rmrp0`                      validation set for GradeNet
    │       `test_seq_n_12_rmrp0`                     test set for GradeNet

    │       `constants.py`                            constants used in preprocessing
    │       `preprocessing_helper.py`                 helper functions for preprocessing

    │       `Step1_data_preprocessing_v2.ipynb`       step 1: 
    │       `Step2_BetaMove.ipynb`                    step 2: 
    │       `Step3_partition_train_test_set_v2.ipynb` step 3: 
    │
    ├───`raw_data` raw data from moonboard
    │       `hold_features_2016.csv`          hold features for both hands? TODO: verify
    │       `hold_features_2016_LH.csv`       hold features for left hand
    │       `hold_features_2016_RH.csv`       hold features for right hand
    │       `holdIx_to_holdStr`               map of hold index to hold string (valid holds only)
    │       `holdStr_to_holdIx`               map of hold string to hold index (valid holds only)
    │       `moonboard2016Background.jpg`     MoonBoard 2016 board image
    │       `MoonBoard_2016_raw.json`         JSON form of `moonGen_scrape_2016_cp`
    │       `MoonBoard_2016_raw_schema.json`  Schema of `MoonBoard_2016_raw.json`
    │       `moonGen_scrape_2016`             Routes w/o hold info
    │       `moonGen_scrape_2016_cp`          Routes w/ hold info
    │       `moonGen_scrape_2016_fail`        Empty
    │       `moonGen_scrape_2016_final`       Routes w/ hold info (different schema)


### Improvements

- imp: model portability
  - `pyproject.toml` for dependency management
- imp: fixed broken sections in preprocessing TODO: specify which sections
- single script `generator.py` for generating problems of a given grade
  - design: how we plan for it to work
  - imp: model saving for performance
  - imp: batch prediction rather than iterative
- imp: fixed loss function used by gradenet for compatability with eager execution

- design: decided not to use class weighting as we want to maintain a similar distribution between our training examples and real world test
  - user are much more likely to submit V4-6 than >V9
  - class weighting resulted in training loss being much greater than validation, due to higher presence of low grades in validation and training
- design: changed gradenet to use regression instead of classification (not ordinal just pure regression, as the grades are numerical)
  - belive this contributed/resulted in the weird confusion matrix
    - as large grade differences were penalised just as much as small differences (due to the same loss)
    - => won't predict close grade
    - => no motivation to predict close grades to desired
  - this adds significant additional information to the loss function as it gives how far off the prediction is from the actual grade

- design: modify deeprouteset to be compatible with generic training boards
  - approximate generalisation
  - explain why and drawbacks

- TODO: modify deeprouteset to generate problems of a given grade
  - add grade as input to model
  - add gradenet to end of model as loss function for similarity to requested grade

ANALYSE AND ATTEMPT THIS based on the 2023 paper
- TODO: design: modify betamove to generate moves from holds using an improved alogrithm
  - currently only uses guassian around current hand positions to evaluate cost
  - best moves are selected via beam search
  - has been suggested to use outlier detection against a set of benchmark moves
  - see the 2023 paper for improved hold difficulty heuristics
  - ensure that the raw data is still present for machine learning along side the added heuristics
