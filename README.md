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

## Train Set

## Second Varient
- Layers
  - model = tf.keras.Sequential()
  - model.add(tf.keras.layers.Dense(512, activation="relu"))
  - model.add(tf.keras.layers.Dense(512, kernel_regularizer=tf.keras.regularizers.l1(0.01)))
  - model.add(tf.keras.layers.Dense(512, 64, kernel_initializer='orthogonal'))
  - model.add(tf.keras.layers.Dense(768, activation="linear"))
- \# Training Steps: 20000
- Batch Size: 32
- Learning Rate: 0.0005
- \# Train Examples: 20000
  For our second trial we added a kernel initializer and a kernel regualizer to our architecture. We changed the hyerparameters of numsteps from 10000 to 20000, and we decreased the learning rate from 0.001 to 0.0005. We did this in the hope that we could get above the default's .67 correct rate, but after about 15000 ish trials, our evaluation does not grow beyond .67
  
2016 validation accuracy: 
0.6584714056654195

2018 validation accuracy: 
0.6575429662635264

2016 test accuracy: 
0.6477819347942276

## Training on Validation Set
### Original Parameters: ###
- Layers
  - (512, activation='relu')
  - (2, activation="linear")
- \# Training Steps: 20000
- Batch Size: 32
- Learning Rate: 0.001
- \# Train Examples: 5000

_Validation accuracy: 0.690_
_2016 test accuracy: 0.7113842864778194_
