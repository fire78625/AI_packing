# AI_ransomware_classification
Record about the performance of our proposed AI model in ransomware classification and the other needed details
Authors: Owen Chen, Ping-Chung Chen

## Training phase
First, each original binary executable in training set needs to use tools or commands to generate 2 category files: .asm file and .byte file

Afterwards, the inputs of training phase are binary executable(PE), .asm file, and .byte file 

The outputs of training phase are 5 models whose file format is .pickle

The categories of output model: Random Forest(RF), XGBoost(XGB), LightGBM(LGB), Pytorch, stack

## Testing phase
Similarly, each binary executable needs to generate .asm file and .byte file at first.

Inputs are the same as training phase. That is, binary executable(PE), .asm file, and .byte file

Output format: Dataframe

## result showing
### single result
![](https://github.com/fire78625/AI_ransomware_classification/blob/main/result_showing/ai_single.png)
### multiple results
![](https://github.com/fire78625/AI_ransomware_classification/blob/main/result_showing/ai_high_classify.png)

![](https://github.com/fire78625/AI_ransomware_classification/blob/main/result_showing/ai_low_classify.JPG)
