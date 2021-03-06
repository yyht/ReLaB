# Reliable Label Bootstrapping for semi-supervised learning (2020): https://arxiv.org/abs/2007.11866
Official implementation.

# How to run
Training on a dataset is sperated in 3 phases.

## First phase (Can be skipped if you already have self-supervised or transfer learning weights for the label propagation)
Train a self-supervised algorithm on the dataset to learn good image descriptors, here we use AND (https://github.com/Raymond-sci/AND). You can alternatively use ImageNet weights (not studied in the paper) by using --load imagenet on main_subset.py. Only ResNet50 supports this feature at the time and results are worse than SSL training.

```sh
$ cd AND
$ bash train_AND.sh
```

## Second phase 
Reliable sample bootstrapping: Propagate the few labels using the previously learned image descriptors and select an extended pool of reliable samples. The bash file also include the semi-supervised training with a Pseudo-Labeling algorithm (https://github.com/EricArazo/PseudoLabeling), you will need to use pretrained RotNet weights with the Pseudo-Labeling algorithm to reach good performance. No self-supervised weights are necessary for ReMixMatch (see below).

```sh
$ cd ..
$ bash train_subset.sh
```

## Final phase
Semi-supervised training on the extended reliable subset, here with ReMixMatch (https://github.com/google-research/remixmatch). The code is in Tensorflow.

```sh
$ cd remixmatch
$ bash train_RMM.sh
```

# Download pretrained weights for AND (Label progation) and for RotNet (Pseudo-Labeling)
https://drive.google.com/drive/folders/1FlUid993oMge6ppdrTmFCK7UhboPi4-c?usp=sharing

# Dependencies (conda)
## ReLaB, AND and Pseudo-Labeling
```sh
$ conda env create -f environment.yml
$ pip install torchcontrib
$ conda activate py3
```

## ReMixMatch (tensorflow)
```sh
$ cd remixmatch
$ conda env create -f condatfenv.yml
$ conda activate tf
```

# Some paper results, refer to arxiv for comparison numbers
These number are from our paper, runs with this cleaned up code can slightly differ, will update them in time...

| Dataset | Labeled Samples | SSL accuracy PL | SSL accuracy RMM |
| ------ | ------ | ------ | ------ |
|CIFAR10|40|83.25|90.65|
|CIFAR10|100|88.59|92.22|
|CIFAR100|400|42.71|51.13|
|CIFAR100|1000|49.36|57.90|
|miniImageNet|400|30.75|-|
|miniImageNet|1000|37.82|-|


# Please cite our paper if it helps your research
```
@misc{albert2020relab,
    title={ReLaB: Reliable Label Bootstrapping for Semi-Supervised Learning},
    author={Paul Albert and Diego Ortego and Eric Arazo and Noel E. O'Connor and Kevin McGuinness},
    year={2020},
    eprint={2007.11866},
    archivePrefix={arXiv},
    primaryClass={cs.CV}
}
```
