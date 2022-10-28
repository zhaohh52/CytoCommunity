# CytoCommunity

## Contents

- [Background](#background)
- [Installation](#installation)
- [Usage](#usage)
- [Maintainers](#maintainers)
- [Contributing](#contributing)
- [License](#license)

## Background

## Installation

### Windows

#### Hardware environment 

CPU: i7

Memory: 16G or more

Storage: 10GB or more

#### Software environment 

Conda version: 22.9.0

Python version: 3.10.6

R version: 4.1.3

#### Preparing the virtual environment

1. Create a new conda environment using .yml file or .txt file with one of the commands:

```
(base) PS D:\test\CytoCommunity-main> conda env create -f environment.yml
(base) PS D:\test\CytoCommunity-main> conda create --name CytoCommunity --file requirements.txt
```

Note that the command should be executed in the parent directory of the .yml or .txt file. If you use the requirements.txt file, please convert it to UTF-8 format.

2. You can also install the requirements directly in a new conda environment via:

```
(base) PS C:\Users\Lenovo> conda create --name CytoCommunity pyhton=3.10.6
(base) PS C:\Users\Lenovo> conda activate CytoCommunity
(CytoCommunity) PS C:\Users\Lenovo> conda install --yes --file requirements.txt
```

### Linux

#### Hardware environment 

CPU: i7

Memory: 16G or more

Storage: 10GB or more

#### Software environment 

Conda version: 22.9.0

Python version: 3.10.6

R version: 4.2.0

#### Preparing the virtual environment 

1. Create a new conda environment for the program and activate it via the commands:

```
(base) conda create --name CytoCommunity python=3.10.6
(base) conda activate CytoCommunity
```

2. Install the dependencies in the exsiting environment directly via:

```
(CytoCommunity) conda install pandas
(CytoCommunity) conda install seaborn
(CytoCommunity) conda install pytorch cpuonly -c pytorch
(CytoCommunity) conda install pyg -c pyg
```

3. Install R and diceR packages via:

```
(CytoCommunity) conda install R
(CytoCommunity) R
> install.packages("diceR")
```

## Usage

CytoCommunity can be utilized through both supervised and unsupervised learning. You can apply CytoCommunity algorithm in the following five steps:

  1. Step0: Constructing the KNN gragh.

  2. Step1: Importing data.

  3. Step2: Performing soft clustering through supervised or unsupervised learning.

  4. Step3: Concensus clustering.

  5. Step4: Visualization of the clustering result.

### Unsupervised CytoCommunity

The input data of the unsupervised learning part of CytoCommunity algorithm is information of MERFISH Brain KNN graph, including a image name list and cell type label, coordinates, edge index, gragh index and node attributes files, seen in the folder "MERFISH_Brain_KNNgraph_Input".

Here comes the running process in the Windows Powershell:

#### 1. Step0_Construct_KNNgraph.py

Use step 0 to construct KNN gragh and prepare data for the following steps.

```
(base) PS C:\Users\Lenovo> conda activate CytoCommunity
(CytoCommunity) PS C:\Users\Lenovo> cd D:\test\CytoCommunity-main\Unsupervised_CytoCommunity
(CytoCommunity) PS D:\test\CytoCommunity-main\Unsupervised_CytoCommunity> python Step0_Construct_KNNgraph.py
```

#### 2. Step1_DataImport.py

Step 1 is for data preprocessing, converting the input data to the standard format of torch. The running result includes two folders, "processed" and "raw", with the former containing three .pt files, named pre_filter, pre_transform and SpatialOmicsImageDataset, and the latter nothing. 

```
(CytoCommunity) PS D:\test\CytoCommunity-main\Unsupervised_CytoCommunity> python Step1_DataImport.py
```

#### 3. Step2_SoftClusterLearning_Unsupervised.py

In step 2, CytoCommunity performs soft clustering through unsupervised learning. This step generates a folder for each epoch of training that contains cluster adjacent matrix, cluster assign matrix, node mask, and gragh index files and a training loss file.

```
(CytoCommunity) PS D:\test\CytoCommunity-main\Unsupervised_CytoCommunity> python Step2_SoftClusterLearning_Unsupervised.py
```

#### 4. Step3_ConsensusClustering.R

To make the soft clustering result more robust, step 3 is consensus clustering using R, and file "ConsensusLabel_MajorityVoting.csv" will be generated to show the result.

```
(CytoCommunity) PS D:\test\CytoCommunity-main\Unsupervised_CytoCommunity> R.exe
> install.packages("diceR")
> source("Step3_ConsensusClustering.R")
```

#### 5. Step4_Visualization.py

Visualization of the consensus clustering result is the final step of CytoCommunity algorithm. After this step, we'll get the gragh of tissue cellular neighborhood(TCN).

```
(CytoCommunity) PS D:\test\CytoCommunity-main\Unsupervised_CytoCommunity> python Step4_Visualization.py
```

### Supervised CytoCommunity

On the other hand, we can also use CytoCommunity algorithm through supervised learning. The input data of this part is information of codex colon cancer KNN graph, including a image name list and cell type label, coordinates, edge index, gragh index, gragh label and node attributes files, seen in the folder "CODEX_ColonCancer_KNNgraph_Input".

Here comes the running process in the Windows Powershell:

#### 1. Step0_Construct_KNNgraph.py

Use step 0 to construct KNN gragh and prepare data for the following steps.

```
(base) PS C:\Users\Lenovo> conda activate CytoCommunity
(CytoCommunity) PS C:\Users\Lenovo> cd D:\test\CytoCommunity-main\Supervised_CytoCommunity
(CytoCommunity) PS D:\test\CytoCommunity-main\Supervised_CytoCommunity> python Step0_Construct_KNNgraph.py
```

#### 2. Step1_DataImport.py

The running result of step 1 includes two folders, "processed" and "raw", with the former containing three .pt files, named pre_filter, pre_transform and SpatialOmicsImageDataset, and the latter nothing.

```
(CytoCommunity) PS D:\test\CytoCommunity-main\Supervised_CytoCommunity> python Step1_DataImport.py
```

#### 3. Step2_SoftClusterLearning_Supervised.py

CytoCommunity uses step 2 to perform soft clustering through supervised learning. For each Fold in each Time of the training process, this step generates a folder that containes cluster adjacent matrix, cluster assign matrix, gragh index, and node mask files and a training loss file.

```
(CytoCommunity) PS D:\test\CytoCommunity-main\Supervised_CytoCommunity> python Step2_SoftClusterLearning_Supervised.py
```

#### 4. Step3_ConsensusClustering.R

In this step, we'll get a image collection folder that contains cluster assign matrix, node mask, gragh index and consensus label files of each Fold in each Time of the training process. 

```
(CytoCommunity) PS D:\test\CytoCommunity-main\Supervised_CytoCommunity> R.exe
> install.packages("diceR")
> source("Step3_ConsensusClustering.R")
```

#### 5. Step4_Visualization.py

After the final step, we'll eventually get the TCN results. 

```
(CytoCommunity) PS D:\test\CytoCommunity-main\Supervised_CytoCommunity> python Step4_Visualization.py
```

## Maintainers

[@HuBioinfo](https://github.com/huBioinfo)(huyuxuan@xidian.edu.cn)

[@yafeixu-xidian](https://github.com/yafeixu-xidian)(22031212416@stu.xidian.edu.cn)

## Contributing

Feel free to dive in!

## License

