# AI_ransomware_classification
record about the performance of our proposed AI model in ransomware classification and the other needed details

## Training phase
First, each original binary executable in training set needs to use tools or commands to generate 2 category files: .asm file and .byte file
Afterwards, the inputs of training phase are binary executable(PE), .asm file, and .byte file 
The outputs of training phase are 5 models whose file format is .pickle
The categories of output model: Random Forest(RF), XGBoost(XGB), LightGBM(LGB), Pytorch, stack

## Testing phase
Similarly, each binary executable needs to generate .asm file and .byte file at first.
Inputs are the same as training phase. That is, binary executable(PE), .asm file, and .byte file
Output format: Dataframe

