# 1. Introduction to Deep Learning in Python
## 1.1 Introduction to deep learning
## 1.2 Forward propagation

```python
# Coding the forward propagation algorithm
node_0_value = (input_data * weights['node_0']).sum() ## Calculate node 0 value: node_0_value
node_1_value = (input_data * weights['node_1']).sum() ## Calculate node 1 value: node_1_value

hidden_layer_outputs = np.array([node_0_value, node_1_value]) ## Put node values into array: hidden_layer_outputs

output = (hidden_layer_outputs * weights['output']).sum() ## Calculate output: output

print(output)
```

## 1.3 Activation functions

```python
# The Rectified Linear Activation Function
def relu(input):
    '''Define your relu activation function here'''
    ## Calculate the value for the output of the relu function: output
    output = max(0, input)
    
    ## Return the value just calculated
    return(output)

## Calculate node 0 value: node_0_output
node_0_input = (input_data * weights['node_0']).sum()
node_0_output = relu(node_0_input)

## Calculate node 1 value: node_1_output
node_1_input = (input_data * weights['node_1']).sum()
node_1_output = relu(node_1_input)

## Put node values into array: hidden_layer_outputs
hidden_layer_outputs = np.array([node_0_output, node_1_output])

## Calculate model output (do not apply relu)
model_output = (hidden_layer_outputs * weights['output']).sum()
print(model_output)
```

```python
# Applying the network to many observations/rows of data
## Define predict_with_network()
def predict_with_network(input_data_row, weights):

    ## Calculate node 0 value
    node_0_input = (input_data_row * weights['node_0']).sum()
    node_0_output = relu(node_0_input)

    ## Calculate node 1 value
    node_1_input = (input_data_row * weights['node_1']).sum()
    node_1_output = relu(node_1_input)

    ## Put node values into array: hidden_layer_outputs
    hidden_layer_outputs = np.array([node_0_output, node_1_output])
    
    ## Calculate model output
    input_to_final_layer = np.array([node_0_output, node_1_output])
    model_output = (hidden_layer_outputs * weights['output']).sum()
    
    ## Return model output
    return(model_output)


## Create empty list to store prediction results
results = []
for input_data_row in input_data:
    ## Append prediction to results
    results.append(predict_with_network(input_data_row, weights))

print(results)        
```

## 1.4 Deeper networks

```python
# Multi-layer neural networks
def predict_with_network(input_data):
    ## Calculate node 0 in the first hidden layer
    node_0_0_input = (input_data * weights['node_0_0']).sum()
    node_0_0_output = relu(node_0_0_input)

    ## Calculate node 1 in the first hidden layer
    node_0_1_input = (input_data * weights['node_0_1']).sum()
    node_0_1_output = relu(node_0_1_input)

    ## Put node values into array: hidden_0_outputs
    hidden_0_outputs = np.array([node_0_0_output, node_0_1_output])
    
    ## Calculate node 0 in the second hidden layer
    node_1_0_input = (hidden_0_outputs * weights['node_1_0']).sum()
    node_1_0_output = relu(node_1_0_input)

    ## Calculate node 1 in the second hidden layer
    node_1_1_input = (hidden_0_outputs * weights['node_1_1']).sum()
    node_1_1_output = relu(node_1_1_input)

    ## Put node values into array: hidden_1_outputs
    hidden_1_outputs = np.array([node_1_0_output, node_1_1_output])

    ## Calculate model output: model_output
    model_output = ((hidden_1_outputs) * weights['output']).sum()
    
    ## Return model_output
    return(model_output)

output = predict_with_network(input_data)
print(output)
```

# 2. Optimizing a neural network with backward propagation
## 2.1 The need for optimization


```python
# Coding how weight changes affect accuracy
## The data point you will make a prediction for
input_data = np.array([0, 3])

## Sample weights
weights_0 = {'node_0': [2, 1],
             'node_1': [1, 2],
             'output': [1, 1]
            }

## The actual target value, used to calculate the error
target_actual = 3

## Make prediction using original weights
model_output_0 = predict_with_network(input_data, weights_0)

## Calculate error: error_0
error_0 = model_output_0 - target_actual

## Create weights that cause the network to make perfect prediction (3): weights_1
weights_1 = {'node_0': [2, 1],
             'node_1': [1, 0],
             'output': [1, 1]
            }

## Make prediction using new weights: model_output_1
model_output_1 = predict_with_network(input_data, weights_1)

## Calculate error: error_1
error_1 = model_output_1 - target_actual

print(error_0)
print(error_1)
```

```python
# Scaling up to multiple data points
from sklearn.metrics import mean_squared_error

model_output_0 = [] ## Create model_output_0  
model_output_1 = [] ## Create model_output_1

## Loop over input_data
for row in input_data:
    ## Append prediction to model_output_0
    model_output_0.append(predict_with_network(row, weights_0))
    
    ## Append prediction to model_output_1
    model_output_1.append(predict_with_network(row, weights_1))

mse_0 = mean_squared_error(target_actuals, model_output_0) ## Calculate the mean squared error for model_output_0: mse_0
mse_1 = mean_squared_error(target_actuals, model_output_1) ## Calculate the mean squared error for model_output_1: mse_1

print("Mean squared error with weights_0: %f" %mse_0)
print("Mean squared error with weights_1: %f" %mse_1)
```

## 2.2 Gradient descent

```python
# Calculating slopes
preds = (input_data * weights).sum() ## Calculate the predictions: preds
error = preds - target ## Calculate the error: error

slope = input_data * error * 2 ## Calculate the slope: slope
print(slope)
```

```python
# Improving model weights
learning_rate = 0.01 ## Set the learning rate: learning_rate

preds = (weights * input_data).sum() ## Calculate the predictions: preds 
error = preds - target ## Calculate the error: error
slope = 2 * input_data * error ## Calculate the slope: slope

weights_updated = weights-(learning_rate * slope) ## Update the weights: weights_updated
preds_updated = (weights_updated * input_data).sum() ## Get updated predictions: preds_updated
error_updated = preds_updated - target ## Calculate updated error: error_updated

print(error)
print(error_updated)
```

```python
# Making multiple updates to weights
n_updates = 20
mse_hist = []

## Iterate over the number of updates
for i in range(n_updates):
    slope = get_slope(input_data, target, weights) ## Calculate the slope: slope    
    weights = weights - slope * 0.01 ## Update the weights: weights
    mse = get_mse(input_data, target, weights) ## Calculate mse with new weights: mse
    
    mse_hist.append(mse) ## Append the mse to mse_hist

plt.plot(mse_hist)
plt.xlabel('Iterations')
plt.ylabel('Mean Squared Error')
plt.show()
```

## 2.3 Backpropagation


# 3. Building deep learning models with keras
## 3.1 Creating a keras model

```python
# Specifying a model
## Import necessary modules
import keras
from keras.layers import Dense
from keras.models import Sequential

## Save the number of columns in predictors: n_cols
n_cols = predictors.shape[1]

## Set up the model: model
model = Sequential()

## Add the first layer
model.add(Dense(50, activation='relu', input_shape=(n_cols,)))

## Add the second layer
model.add(Dense(32, activation='relu', input_shape=(n_cols,)))

## Add the output layer
model.add(Dense(1, input_shape=(n_cols,)))
```

## 3.2 Compiling and fitting a model

```python
# Compiling the model
## Import necessary modules
import keras
from keras.layers import Dense
from keras.models import Sequential

## Specify the model
n_cols = predictors.shape[1]
model = Sequential()
model.add(Dense(50, activation='relu', input_shape = (n_cols,)))
model.add(Dense(32, activation='relu'))
model.add(Dense(1))

## Compile the model
model.compile(optimizer = 'adam', loss = 'mean_squared_error')

## Verify that model contains information from compiling
print("Loss function: " + model.loss)
```

```python
# Fitting the model
## Import necessary modules
import keras
from keras.layers import Dense
from keras.models import Sequential

## Specify the model
n_cols = predictors.shape[1]
model = Sequential()
model.add(Dense(50, activation='relu', input_shape = (n_cols,)))
model.add(Dense(32, activation='relu'))
model.add(Dense(1))

## Compile the model
model.compile(optimizer='adam', loss='mean_squared_error')

## Fit the model
model.fit(predictors, target)
```

## 3.3 Classification models

```python
# Last steps in classification models
## Import necessary modules
import keras
from keras.layers import Dense
from keras.models import Sequential
from keras.utils import to_categorical

## Convert the target to categorical: target
target = to_categorical(df.survived)

## Set up the model
model = Sequential()

## Add the first layer
model.add(Dense(32, activation='relu', input_shape = (n_cols,)))

## Add the output layer
model.add(Dense(2, activation='softmax', input_shape = (n_cols,)))

## Compile the model
model.compile(optimizer='sgd', loss='categorical_crossentropy', metrics=['accuracy'])

## Fit the model
model.fit(predictors, target)
```

## 3.4 Using models

```python
# Making predictions
## Specify, compile, and fit the model
model = Sequential()
model.add(Dense(32, activation='relu', input_shape = (n_cols,)))
model.add(Dense(2, activation='softmax'))
model.compile(optimizer='sgd', 
              loss='categorical_crossentropy', 
              metrics=['accuracy'])
model.fit(predictors, target)

## Calculate predictions: predictions
predictions = model.predict(pred_data)

## Calculate predicted probability of survival: predicted_prob_true
predicted_prob_true = predictions[:,1]
print(predicted_prob_true)
```

# 4. Fine-tuning keras models
## 4.1 Understanding model optimization

```python
# Changing optimization parameters
## Import the SGD optimizer
from keras.optimizers import SGD

## Create list of learning rates: lr_to_test
lr_to_test = [.000001, 0.01, 1]

## Loop over learning rates
for lr in lr_to_test:
    print('\n\nTesting model with learning rate: %f\n'%lr )
    
    ## Build new model to test, unaffected by previous models
    model = get_new_model()
    
    ## Create SGD optimizer with specified learning rate: my_optimizer
    my_optimizer = SGD(lr=lr)
    
    ## Compile the model
    model.compile(optimizer = my_optimizer, loss = 'categorical_crossentropy') 
    
    ## Fit the model
    model.fit(predictors, target)
```

## 4.2 Model validation

```python
# Evaluating model accuracy on validation dataset
## Save the number of columns in predictors: n_cols
n_cols = predictors.shape[1]
input_shape = (n_cols,)

## Specify the model
model = Sequential()
model.add(Dense(100, activation='relu', input_shape = input_shape))
model.add(Dense(100, activation='relu'))
model.add(Dense(2, activation='softmax'))

## Compile the model
model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])

## Fit the model
hist = model.fit(predictors, target, validation_split=0.3)
```

```python
# Early stopping: Optimizing the optimization
## Import EarlyStopping
from keras.callbacks import EarlyStopping

## Save the number of columns in predictors: n_cols
n_cols = predictors.shape[1]
input_shape = (n_cols,)

## Specify the model
model = Sequential()
model.add(Dense(100, activation='relu', input_shape = input_shape))
model.add(Dense(100, activation='relu'))
model.add(Dense(2, activation='softmax'))

## Compile the model
model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])

## Define early_stopping_monitor
early_stopping_monitor = EarlyStopping(patience=2)

## Fit the model
model.fit(predictors, target, epochs=30, validation_split=0.3, callbacks=[early_stopping_monitor])
```

```python
# Experimenting with wider networks
## Define early_stopping_monitor
early_stopping_monitor = EarlyStopping(patience=2)

## Create the new model: model_2
model_2 = Sequential()

## Add the first and second layers
model_2.add(Dense(100, activation='relu', input_shape=input_shape))
model_2.add(Dense(100, activation='relu', input_shape=input_shape))

## Add the output layer
model_2.add(Dense(2, activation='softmax'))

## Compile model_2
model_2.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])

## Fit model_1
model_1_training = model_1.fit(predictors, target, epochs=15, validation_split=0.2, callbacks=[early_stopping_monitor], verbose=False)

## Fit model_2
model_2_training = model_2.fit(predictors, target, epochs=15, validation_split=0.2, callbacks=[early_stopping_monitor], verbose=False)

## Create the plot
plt.plot(model_1_training.history['val_loss'], 'r', model_2_training.history['val_loss'], 'b')
plt.xlabel('Epochs')
plt.ylabel('Validation score')
plt.show()
```

```python
# Adding layers to a network
## The input shape to use in the first hidden layer
input_shape = (n_cols,)

## Create the new model: model_2
model_2 = Sequential()

## Add the first, second, and third hidden layers
model_2.add(Dense(50, activation='relu', input_shape=input_shape))
model_2.add(Dense(50, activation='relu', input_shape=input_shape))
model_2.add(Dense(50, activation='relu', input_shape=input_shape))

## Add the output layer
model_2.add(Dense(2, activation='softmax'))

## Compile model_2
model_2.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])

## Fit model 1
model_1_training = model_1.fit(predictors, target, epochs=20, validation_split=0.4, callbacks=[early_stopping_monitor], verbose=False)

## Fit model 2
model_2_training = model_2.fit(predictors, target, epochs=20, validation_split=0.4, callbacks=[early_stopping_monitor], verbose=False)

## Create the plot
plt.plot(model_1_training.history['val_loss'], 'r', model_2_training.history['val_loss'], 'b')
plt.xlabel('Epochs')
plt.ylabel('Validation score')
plt.show()
```

## 4.3 Stepping up to images

```python
# Building your own digit recognition model
model = Sequential() ## Create the model: model

model.add(Dense(50, activation='relu', input_shape=(784,))) ## Add the first hidden layer
model.add(Dense(50, activation='relu', input_shape=(784,))) ## Add the second hidden layer
model.add(Dense(10, activation='softmax')) ## Add the output layer

## Compile the model
model.compile(optimizer='adam', loss='categorical_crossentropy', metrics=['accuracy'])

## Fit the model
model.fit(X, y, validation_split = 0.3)
```