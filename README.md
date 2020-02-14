# cis700-text-adventures-hw3
Reasoning about Story Endings
-----------------------------

## Sentiment Analysis
### Methods
In order to utilize the sentiment analysis library functions, I used the linked website to [AllenNLP](https://demo.allennlp.org/sentiment-analysis/MTQxOTUzNw==) and used the "AS A LIBRARY (PYTHON): code block. After importing the library, you should set the `predictor` variable. To use the `predictor.predict()` function, I printed out the results of the predict function and look at how the `data` parameter would be formatted to extract the necessary information. 

### Results
_2016 validation accuracy: 0.6007482629609834_

_2018 validation accuracy: 0.5945257797581158_

_2016 test accuracy: 0.5921966862640299_

The starting test accuracy was not amazing at about 60%, but using sentiment and only returning the more positive sentiment and not getting a 50% actually shows a bias for positive next sentences in the examples. This was already mentioned in the description, and it was said that the 2018 dataset attempted to eliminate the bias. But from the 2018 validation accuracy, it seems to have dipped only slightly. This may not be the fault of the 2018 dataset, however, because from testing the sentiment analysis given by AllenNLP, we saw that it would deem more neutral statements to be positive still. But there is still the point that the "positive" probability is still a decimal that would be comparable, so there likely may still exist a bias towards positive next sentences.

I also tried averaging the context sentences' sentiment and comparing the distances between the computed sentiment average and each option's sentiments, but I met with worse results. This is likely because the contexts of the data entries didn't really correlate much with the option's sentiments.

## Training on the Train Set
### Methods
We looked at the given link to the _keras.layer_ link to see the different parameters we could configure to test for different accuracies. We didn't have a particular method of determining which to configure. However, we realized that configuring just one aspect of the model would be more helpful in pinning down what helped the model improve.

### Results
#### Original Parameters
- Layers
~~~~
  model = tf.keras.Sequential()
  model.add(tf.keras.layers.Dense(512, activation="relu"))
  model.add(tf.keras.layers.Dense(768, activation="linear"))
~~~~
- \# Training Steps: 10000
- Batch Size: 32
- Number of Candidates: 64
- Learning Rate: 0.001
- \# Train Examples: 5000

_2016 validation accuracy: 0.6392303580972741_

_2018 validation accuracy: 0.6371737746658179_

_2016 test accuracy: 0.6456440406199893_

#### First Variant:
- Layers
~~~~
  model = tf.keras.Sequential()
  model.add(tf.keras.layers.Dense(512, activation="relu"))
  model.add(tf.keras.layers.Dense(64, kernel_regularizer=tf.keras.regularizers.l1(0.01)))
  model.add(tf.keras.layers.Dense(768, activation="linear"))
~~~~
- \# Training Steps: 20000
- Batch Size: 32
- Number of Candidates: 64
- Learning Rate: 0.001
- \# Train Examples: 5000

_2016 validation accuracy: 0.5098877605558525_

_2018 validation accuracy: 0.5111394016549968_

_2016 test accuracy: 0.518439337252806_

For our first trial we added a kernel regulator, and increased the number of training steps. Somehow, this model did worse. We had assumed that adding new layers and increasing step size would at the very worst not change anything, not drastically worsen the model. So that is pretty interesting. Increasing the number training steps may have caused the resulting calculations to both overshoot and undershoot predictions. Adding the kernel regulator would apply penalties on the layer during optimization, which are incorporated in the loss function, so the kernel regulator coupled with the increased steps may have caused more penalties than necessary, which resulted in a lower accuracy.

#### Second Variant:
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
- Number of Candidates: 64
- Learning Rate: 0.0005
- \# Train Examples: 20000

_2016 validation accuracy: 0.6584714056654195_

_2018 validation accuracy: 0.6575429662635264_

_2016 test accuracy: 0.6477819347942276_

For our second trial we added a kernel initializer and a kernel regulizer to our architecture. We changed the hyerparameters of numsteps from 10000 to 20000, and we decreased the learning rate from 0.001 to 0.0005. We did this in the hope that we could get above the default's .67 correct rate, but after about 15000 ish trials, our evaluation does not grow beyond .67. Decreasing the learning rate may have helped the accuracy rate because even with the increased number of steps, the lower learning rate likely helped the calculations to fluctuate less with a smaller margin of change and decrease the number of penalties.

## Training on Validation Set

### Methods
Same as above!

#### Original Parameters: ###
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
- Learning Rate: 0.0001
- \# Train Examples: 5000

_2016 test accuracy: 0.7268840192410476_

Here, we thought that decreasing the learning rate would give us a better model. Because even though the change is smaller with every step, the change we do get is more accurate. It looks like we were correct in this assumption. Since the number of training steps when we test the validation dataset is much greater than just the testing set, decreasing the learning rate must have helped the model hone in closer to the right calculations.

### Second Variant:
- Layers
~~~~
  model = tf.keras.Sequential()
  model.add(tf.keras.layers.Dense(512, activation="relu"))
  model.add(tf.keras.layers.Dense(2, activation="linear"))
~~~~
- \# Training Steps: 40000
- Batch Size: 32
- Learning Rate: 0.0005
- \# Train Examples: 5000

_2016 test accuracy: 0.7493319080705505_

We thought that increasing number of steps and decreasing learning rate would yeild a more accurate model. We were correct in this assumption. However, in previous undocumented case, we created a model with the same parameters, but we added an extra layer to the architecture in the form of a bias regulator. This change actually drastically decreased the accuray of the model, indicating that increasing architecture complexity does not necessarily yield better results. But we may not have used the correct layers as well.

