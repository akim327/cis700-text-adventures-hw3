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
### Original Parameters
- Layers
~~~~
  model = tf.keras.Sequential()
  model.add(tf.keras.layers.Dense(512, activation="relu"))
  model.add(tf.keras.layers.Dense(768, activation="linear"))
~~~~
- \# Training Steps: 10000
- Batch Size: 32
- Learning Rate: 0.001
- \# Train Examples: 5000

_2016 validation accuracy: 0.6392303580972741

_2018 validation accuracy: 0.6371737746658179

_2016 test accuracy: 0.6456440406199893

### First Variant:
- Layers
~~~~
  model = tf.keras.Sequential()
  model.add(tf.keras.layers.Dense(512, activation="relu"))
  model.add(tf.keras.layers.Dense(64, kernel_regularizer=tf.keras.regularizers.l1(0.01)))
  model.add(tf.keras.layers.Dense(768, activation="linear"))
~~~~
- \# Training Steps: 20000
- Batch Size: 32
- Learning Rate: 0.001
- \# Train Examples: 5000

_2016 validation accuracy: 0.5098877605558525_

_2018 validation accuracy: 0.5111394016549968_

_2016 test accuracy: 0.518439337252806_

### Second Variant:
- Layers
~~~~
  model = tf.keras.Sequential()
  model.add(tf.keras.layers.Dense(512, activation="relu"))
  model.add(tf.keras.layers.Dense(512, kernel_regularizer=tf.keras.regularizers.l1(0.01)))
  model.add(tf.keras.layers.Dense(512, 64, kernel_initializer='orthogonal'))
  model.add(tf.keras.layers.Dense(768, activation="linear"))
~~~~
- \# Training Steps: 20000
- Batch Size: 32
- Learning Rate: 0.0005
- \# Train Examples: 20000
  For our second trial we added a kernel initializer and a kernel regualizer to our architecture. We changed the hyerparameters of numsteps from 10000 to 20000, and we decreased the learning rate from 0.001 to 0.0005. We did this in the hope that we could get above the default's .67 correct rate, but after about 15000 ish trials, our evaluation does not grow beyond .67

_2016 validation accuracy: 0.6584714056654195_

_2018 validation accuracy: 0.6575429662635264_

_2016 test accuracy: 0.6477819347942276_

## Training on Validation Set
### Original Parameters: ###
- Layers
~~~~
  model = tf.keras.Sequential()
  model.add(tf.keras.layers.Dense(512, activation="relu"))
  model.add(tf.keras.layers.Dense(2, activation="linear"))
~~~~
- \# Training Steps: 20000
- Batch Size: 32
- Learning Rate: 0.001
- \# Train Examples: 5000

_2016 test accuracy: 0.7113842864778194_

### First Variant:
- Layers
~~~~
  model = tf.keras.Sequential()
  model.add(tf.keras.layers.Dense(512, activation="relu"))
  model.add(tf.keras.layers.Dense(2, activation="linear"))
~~~~
- \# Training Steps: 20000
- Batch Size: 32
- Learning Rate: 0.1
- \# Train Examples: 5000

_2016 test accuracy: 0.5130946018172

### Second Variant:
- Layers
~~~~
  model = tf.keras.Sequential()
  model.add(tf.keras.layers.Dense(512, activation="relu"))
  model.add(tf.keras.layers.Dense(64, bias_regularizer=tf.keras.regularizers.l2(0.01)))
  model.add(tf.keras.layers.Dense(2, activation="linear"))
~~~~
- \# Training Steps: 40000
- Batch Size: 32
- Learning Rate: 0.0005
- \# Train Examples: 5000

_2016 test accuracy: 0.5130946018172
