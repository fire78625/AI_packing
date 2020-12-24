# PE_malware - AI_ransomware_classification
Introduce the cencept of our model which was used to attend the AI competition and won 2nd place

Moreover, showing how we applied it to classify ransomware samples with our own defined categories and the results 

Authors: Owen Chen, Ping-Chung Chen, Chih-Wei Chen

Source category:

(1) Source code of training and testing (/source_code)

(2) Example samples for testing (/testsample)

(3) Figures of execution results (/result_showing)

## Introduction

The original model attends the AI competition whose name is "2019 SecBuzzer AI UP! AI Information Security Challenge" which was hold by III (Institute For Information Industry)

Index page of AI competition: https://competition.secbuzzer.co/

The evaluation standard of competition is the accuracy of classification, and our model holds at least 50% accuracy to each category of malware

### Analysis process

Use feature extracting techniques: Select Hex code, OpCode, and Hex image to extract features.
Then an AI model which is trained by these features will be used to identify the category of malware.

![](https://github.com/fire78625/AI_ransomware_classification/blob/main/result_showing/malware_analysis.JPG)

(1) Hex code

Find features from Hex file, and use model to find relation between features and functions
![](https://github.com/fire78625/AI_ransomware_classification/blob/main/result_showing/hex_code_analysis.JPG)

(2) OpCode

Using IDA Pro to convert the .asm file of OpCode, then use keywords analysis to transfer Opcodes to Vector
![](https://github.com/fire78625/AI_ransomware_classification/blob/main/result_showing/opcode_analysis.JPG)

(3) Hex image

Transfer Hex code into gray image, then use Fourier transform to describe better features of image
![](https://github.com/fire78625/AI_ransomware_classification/blob/main/result_showing/hex_image_analysis.JPG)

### Diagram of model

Using stacking model to training and testing, then merging the results of different models to balance result
![](https://github.com/fire78625/AI_ransomware_classification/blob/main/result_showing/stack_model.JPG)

### Classification result

Accuracy > 50% in each category of malware
![](https://github.com/fire78625/AI_ransomware_classification/blob/main/result_showing/accuracy.JPG)

## Experimental environment setting
### Operation System: Ubuntu 16.04 LTS Client (or at least Linux-based system recommended)
### install steps:

(1) Install anaconda from Offical website: https://docs.anaconda.com/anaconda/install/windows/

(2) Construct virtual environment by anaconda: https://medium.com/python4u/%E7%94%A8conda%E5%BB%BA%E7%AB%8B%E5%8F%8A%E7%AE%A1%E7%90%86python%E8%99%9B%E6%93%AC%E7%92%B0%E5%A2%83-b61fd2a76566 [python version >= 3]

(3) Check tools version from environment.txt in directory source_code. Most tools should have been installed when virtual environment construct success
!!! We recommend to install the same version of tools to avoid unexcepted conflicts

You can check by open example.ipynb and execute the first 4 cell to confirm that do confilcts exist or not 

### Categories (self-definition) & samples amount in training set

We manually classify the ransomware into 10 categories according to the names from Kaspersky in VirusTotal

Category name                      |          amount

Trojan-Ransom.Win32.Blocker.xxx       |       1103

Trojan-Ransom.Win32.GandCrypt.xxx     |       911

Trojan-Ransom.Win32.Foreign.xxx       |       317

Trojan-Ransom.Win32.PornoBlocker.xxx  |       63

Trojan-Ransom.Win32.Wanna.xxx         |       163

HEUR:Trojan-Ransom.xxx.Blocker.gen    |       110

HEUR:Trojan-Ransom.xxx.Gen.gen        |       14

Trojan-Ransom.PHP.xxx                 |       1

HEUR:Trojan-Ransom.Win32.xxx.vho      |       65

Others                                |       4886  (72.3%)

 
## Usage

Extract souce code from the zip file 'ML_model.zip' first 

### Training phase
First, each original binary executable in training set needs to use tools (ex: IDA Pro) or commands (ex: xxd) to generate 2 category files: .asm file and .byte file

If you use IDA Pro to convert asm file, you can use this command: idaq64 -c -B <targetfile_path> -o<output_dir_path>

!! If you use -o parameter, your OS need GUI interface to make the processing normally

Please put your binary executable in 'bin' directory 

#### Executing the following command to generate bytefile [output byte file is in 'byte' directory]

python3 filterbyte.py 

[Notice it used Linux command xxd, so need to execute in Linux-based environment]

Afterwards, the needed inputs of training phase are binary executable(PE), .asm file, and .byte file [Notice that we assume your binary filename is like <xxx>.<extname>]

The outputs of training phase are 5 models whose file format is .pickle

The categories of output model: Random Forest(RF), XGBoost(XGB), LightGBM(LGB), Pytorch, stack

#### Training steps:

Notice we set default asm files in 'asm' directory, byte files in 'byte' directory, and binary files in 'bin' directory

If you want to change the target directory, please modify corresponding code parameters. Otherwise, please put the files to their corresponding directory

(1) Feature generate stage [data format: csv]

Using extract_feature.ipynb to generate a file which record the extracted features of binary files (1st ~ 6th cell). First parameter of sample should be filename(without extname) and the others are the extracted features in order [Needed inputs are binary executable, .asm file, and .byte file. Output is a csv file]

#### Executing the following command to generate features file: 

python3 extract_feature.py

!!! Notice there are two possible error files in 'errordir' directory

One of their name is err_VT_train_bytelist, it record the error msg during training process. The error in this process often occurs in bytefile

The other name is err_VT_training_feature, it record the error filename. This error occurs because the extracted features amount does not match the number we assumed. And this will cause the dimension problem when training. Thus, we abandon these files.


    
(2) Label generate stage [record format: csv]

##### Execute the following command to generate label file: [only extract the label of Ransomware, input parameter is the Virustotal report name and its format should be a json file]

python3 Samplelabel.py <VirusTotal_report_name>

output is located on 'label' directory and filename = <yourreportname>_label_record.txt

(3) Training model

##### Execute the following command to training model: [outputs are 5 models in the directory 'model']

python3 model_training.py <features_file_name> <label_file_name> <amount of your category>

### Training sample list

We provide the md5 list of our training samples which download from VirusTotal in directory source_code

### Testing phase
Similarly, each binary executable needs to generate .asm file (ex: IDA Pro) and .byte (ex: xxd) file at first.

Inputs are the same as training phase. That is, binary executable(PE), .asm file, and .byte file

Output format: ndarray

#### Testing steps:

##### Execute the following command to testing single sample: [output is a ndarray]

python3 Example.py <bytesfile_path> <asmfile_path> 

### Testing sample

We provide a zip file testsample.zip which contains 10 testing samples in directory testsample. 

#### Testing result

If you follow the testing steps, and you can acquire a response whose format is Dataframe. The visualization of response is displayed below.

The left column list the names of category, the right column are the values of possibility of prediction. The largest value is the prediction result

![](https://github.com/fire78625/AI_ransomware_classification/blob/main/result_showing/ai_single.jpg)

