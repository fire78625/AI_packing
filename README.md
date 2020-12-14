# PE_malware - AI_ransomware_classification
Introduce the cencept of our model which was used to attend the AI competition and won 2nd place

Moreover, showing how we applied it to classify ransomware samples with our own defined categories and the results 

Authors: Owen Chen, Ping-Chung Chen, Chih-Wei Chen

source category:
(1) Source code of training and testing (/source_code)

(2) Example samples for testing (/testsample)

(3) Figures of execution results (/result_showing)

## Introduction

The original model attends the AI competition whose name is "2019 SecBuzzer AI UP! AI Information Security Challenge" which was hold by III

Its evaluation standard is the accuracy of classification

### Analysis process

Use feature extracting techniques: Select Hex code, Op Code, and Hex image to extract features
![](https://github.com/fire78625/AI_ransomware_classification/blob/main/result_showing/malware_analysis.JPG)

(1) Hex code

Find features from Hex file, and use model to find relation between features and functions
~[](https://github.com/fire78625/AI_ransomware_classification/blob/main/result_showing/hex_code_analysis.JPG)

(2) OpCode

Using IDA Pro to convert the .asm file of OpCode, then use keywords analysis to transfer Opcodes to Vector
~[](https://github.com/fire78625/AI_ransomware_classification/blob/main/result_showing/opcode_analysis.JPG)

(3) Hex image

Transfer Hex code into gray image, then use Fourier transform to describe better features of image
~[](https://github.com/fire78625/AI_ransomware_classification/blob/main/result_showing/hex_code_analysis.JPG)

### Diagram of model

Using stacking model to training and testing, then merging the results of different models to balance result
~[](https://github.com/fire78625/AI_ransomware_classification/blob/main/result_showing/stack_model.JPG)

### Experimental result

Accuracy > 50%
~[](https://github.com/fire78625/AI_ransomware_classification/blob/main/result_showing/accuracy.JPG)

## Training phase
First, each original binary executable in training set needs to use tools or commands to generate 2 category files: .asm file and .byte file

Afterwards, the inputs of training phase are binary executable(PE), .asm file, and .byte file 

The outputs of training phase are 5 models whose file format is .pickle

The categories of output model: Random Forest(RF), XGBoost(XGB), LightGBM(LGB), Pytorch, stack

###Training steps:

(1) Feature generate stage
    Using .ipynb to generate a file which record the extracted features of binary files. First parameter should be filename(without extname) and 
## Testing phase
Similarly, each binary executable needs to generate .asm file and .byte file at first.

Inputs are the same as training phase. That is, binary executable(PE), .asm file, and .byte file

Output format: Dataframe

## Result showing

### Experimental environment setting
#### Operation System: Ubuntu 16.04 LTS Client (or at least Linux-based system recommended)
#### install steps:

(1) Install anaconda from Offical website: https://docs.anaconda.com/anaconda/install/windows/

(2) Construct virtual environment by anaconda: https://medium.com/python4u/%E7%94%A8conda%E5%BB%BA%E7%AB%8B%E5%8F%8A%E7%AE%A1%E7%90%86python%E8%99%9B%E6%93%AC%E7%92%B0%E5%A2%83-b61fd2a76566 [python version >= 3]

(3) Check tools version from environment.txt in directory source_code. Most tools should have been installed when virtual environment construct success
!!! We recommend to install the same version of tools to avoid unexcepted conflicts

You can check by open example.ipynb and execute the first 4 cell to confirm that do confilcts exist or not 

### single result
![](https://github.com/fire78625/AI_ransomware_classification/blob/main/result_showing/ai_single.png)
### multiple results
![](https://github.com/fire78625/AI_ransomware_classification/blob/main/result_showing/ai_high_classify.png)

![](https://github.com/fire78625/AI_ransomware_classification/blob/main/result_showing/ai_low_classify.JPG)
