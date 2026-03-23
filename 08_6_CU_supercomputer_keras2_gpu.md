## Install tool chain for keras v2 on Alpine GPU

This install is for both **Python** and **R**. This workflow was created in March 2025 and tested in March 2026.

Request transfer to a GPU compute node for 60 mins.

```bash
sinteractive --partition=atesting_a100 --time=0:60:00 --nodes=1 --ntasks=4 --gres=gpu:1 --qos=testing
```

Your prompt will change to something like:

```
[jbsmith@c3gpu-c2-u13 ~]$
```

But the prompt might also be in an odd part of the screen. Type `clear` to refresh the terminal. Start the Anaconda module

```bash
module load anaconda
```

Your prompt will change to something like:

```
(base) [jbsmith@c3gpu-c2-u13 ~]$
```

indicating that you are in the base conda environment. We now want to set up an environment specifically for R, keras and tensorflow. What we are doing is installing all of the necessary software into an isolated computing environment. First create a new conda environment:

```bash
conda create --name r-tf2150py3118
```

You may see a warning that conda needs to be updated. We don't have control of this. The version of conda is managed by the supercomputer admins. Ignore the warning. You will be asked to confirm to proceed with creating the new environment. Choose yes.

Now activate that environment

```bash
conda activate r-tf2150py3118
```

You should see the environment change in your prompt

```
(r-tf2150py3118) [jbsmith@c3gpu-c2-u13 ~]$
```

Now install all the needed software. This may take as long as 20 minutes.

```bash
conda install r python=3.11.8 tensorflow-gpu=2.15.0 tensorflow-hub tensorflow-datasets scipy requests Pillow h5py pandas pydot -c conda-forge
```



## Check Python keras

Start Python by typing `python` at the prompt.  Work through this example to check that everything is working. This is the canonical keras example. To use keras in the future you'll need to activate the conda environment we set up above.

```python
import tensorflow as tf #there will be several warnings that can be ignored
from tensorflow.keras.utils import to_categorical
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Dropout
from tensorflow.keras.optimizers import RMSprop

# Check tensorflow GPU configuration. Should list and name all 
# GPU devices with status TRUE
print("Num GPUs Available:", len(tf.config.list_physical_devices('GPU')))
print(tf.config.list_physical_devices('GPU'))

# Mnist handwritten letters dataset
(x_train, y_train), (x_test, y_test) = tf.keras.datasets.mnist.load_data()

# reshape
x_train = x_train.reshape(x_train.shape[0], 784).astype("float32")
x_test  = x_test.reshape(x_test.shape[0], 784).astype("float32")

# rescale
x_train /= 255.0
x_test  /= 255.0

# recode
y_train = to_categorical(y_train, 10)
y_test  = to_categorical(y_test, 10)

# Define, compile and fit the model (2 layer feedforward network)
model = Sequential([
    Dense(256, activation="relu", input_shape=(784,)),
    Dropout(0.4),
    Dense(128, activation="relu"),
    Dropout(0.3),
    Dense(10, activation="softmax")
])

model.compile(
    loss="categorical_crossentropy",
    optimizer=RMSprop(),
    metrics=["accuracy"]
)

model.summary()

# Fit should use GPU. It will take a few moments to set up the GPU the first time:
model.fit(
    x_train, y_train,
    batch_size=128,
    epochs=30,
    validation_split=0.2
)

weights = model.get_weights()
print("Number of tensors:", len(weights)) #should be 6
```

When you've finished with Python, type `quit()`. Continue to the next section and test R. 



## Install and check R keras

Start R by typing `R` at the prompt. Install the `keras` library. This will install the package for keras 2, which is what we want for now (keras3 is not compatible with the version of tensorflow we installed above).

```R
install.packages("keras")
```

You will need to install any other R packages you want to use, such as`dplyr`, the same way.

Now we can use the R keras library. Work through this example to check that everything is working. This is the canonical keras example.

```R
# Tell reticulate which conda environment to use
reticulate::use_condaenv(condaenv = "r-tf2150py3118")

# Check tensorflow GPU configuration. Should list and name all 
# GPU devices with status TRUE
tensorflow::tf_gpu_configured(verbose = TRUE)

# Now you can load keras (must be after `use_condaenv` above)
library(keras)

# Mnist handwritten letters dataset
mnist <- dataset_mnist()
x_train <- mnist$train$x
y_train <- mnist$train$y
x_test <- mnist$test$x
y_test <- mnist$test$y

# reshape
x_train <- array_reshape(x_train, c(nrow(x_train), 784))
x_test <- array_reshape(x_test, c(nrow(x_test), 784))

# rescale
x_train <- x_train / 255
x_test <- x_test / 255

# recode
y_train <- to_categorical(y_train, 10)
y_test <- to_categorical(y_test, 10)

# Define, compile and fit the model (2 layer feedforward network)
model <- keras_model_sequential(input_shape = c(784)) |>
  layer_dense(units = 256, activation = 'relu') |>
  layer_dropout(rate = 0.4) |>
  layer_dense(units = 128, activation = 'relu') |>
  layer_dropout(rate = 0.3) |>
  layer_dense(units = 10, activation = 'softmax')

compile(model, loss='categorical_crossentropy', optimizer=optimizer_rmsprop(),
        metrics=c('accuracy'))

print(model)

# Fit should use GPU. It will take a few moments to set up the GPU the first time:
fit(model, x_train, y_train, epochs=30, batch_size=128, validation_split=0.2)

weights <- get_weights(model)
paste("Number of tensors:", length(weights)) #should be 6
```

When you've finished with R, type `q()` and choose "no" to not save the workspace image.

To end your session on the compute node, type `exit`. This will bring you back to the login node. You might want to type `clear` to clear the screen as the prompt will often jump to somewhere randomly in the printed output. To logout, type `logout`.
