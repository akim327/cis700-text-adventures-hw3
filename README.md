# cis700-text-adventures-hw3
Reasoning about Story Endings
-----------------------------

## Sentiment Analysis
2016 validation accuracy: 
0.6007482629609834

2018 validation accuracy: 
0.5945257797581158

2016 test accuracy: 
0.5921966862640299

## Training on the Train Set
### Original Parameters: ###
- Layers
~~~~
(512, activation="relu")
(64, kernel_regularizer=tf.keras.regularizers.l1(0.01))
(768, activation="linear")
~~~~
- \# Training Steps: 20000
- Batch Size: 32
- Learning Rate: 0.001
- \# Train Examples: 5000

_2016 validation accuracy: 0.5098877605558525_
_2018 validation accuracy: 0.5111394016549968_
_2016 test accuracy: 0.518439337252806_

## Training on Validation Set
### Original Parameters: ###
- Layers
~~~~
(512, activation='relu')
(2, activation="linear")
~~~~
- \# Training Steps: 20000
- Batch Size: 32
- Learning Rate: 0.001
- \# Train Examples: 5000

_2016 test accuracy: 0.7113842864778194_
