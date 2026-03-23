## Install tool chain for keras v3 on Alpine GPU

Keras is installed by installing tensorflow. I used the tensorflow installation instructions [here](https://www.tensorflow.org/install) and [here](https://www.tensorflow.org/install/pip#step-by-step_instructions). Workflow created 22 Mar 2026.

Request transfer to a GPU compute node for 60 mins. Give yourself an hour, as conda might need it!

```bash
sinteractive --partition=atesting_a100 --time=0:60:00 --nodes=1 --ntasks=8 --gres=gpu:1 --qos=testing
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

indicating that you are in the base conda environment. We now want to set up an environment specifically for keras 3, including R, Python, and Tensorflow. What we are doing is installing all of the necessary software into an isolated computing environment. First create a new conda environment:

```bash
conda create --name r-py-keras3
```

You may see a warning that conda needs to be updated. We don't have control of this. The version of conda is managed by the supercomputer admins. Ignore the warning. You will be asked to confirm to proceed with creating the new environment. Choose yes.

Now activate the environment

```bash
conda activate r-py-keras3
```

You should see the environment change in your prompt

```
(r-py-keras3) [jbsmith@c3gpu-c2-u13 ~]$
```

To install the `keras3` package in R, we need an appropriate C++ build environment. Check the supercomputer's version of `glibc`:

```bash
ldd --version
```

To ensure that the build environment is compatible with the supercomputer, pick a `sysroot_linux-64` version ≤ the supercomputer host version. At the time of writing, it was 2.28. This version is available from conda-forge according to this search:

```bash
conda search sysroot_linux-64 -c conda-forge
```

Install the C++ build environment, R and Python 3.11, which is the Python version currently compatible with `keras3` in R. Be patient. This may sometimes seem as though nothing is happening and it may take 15 mins or more

```bash
conda install compilers cxx-compiler sysroot_linux-64=2.28 \
    libstdcxx-ng zlib pkg-config r-base python=3.11 -c conda-forge
```

The `zlib` and `pkg-config` libraries are needed for some R packages required by the `keras3` R package (I worked this out by trying to install `keras3` and inspecting the error messages when compilation of a package failed; installation of system dependencies is often needed on linux systems). 

Now install tensorflow. Again, be patient. This might take 20 mins or more.

```bash
pip install tensorflow[and-cuda]==2.16.2
```

At the time of writing these instructions, the above installed R 4.5.3, Python 3.11.15, pip 26.0.1 (a Python package manager), and Keras 3.13.2. You can see the installed versions with

```bash
conda list
```



## Check Python keras

Start Python by typing `python` at the prompt.  Work through this example to check that everything is working. This is the canonical keras example. To use keras in the future you'll need to activate the conda environment we set up above.

```python
import tensorflow as tf #there will be several warnings that can be ignored
from tensorflow.keras.utils import to_categorical
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Dropout, Input
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
    Input(shape=(784,)),
    Dense(256, activation="relu"),
    Dropout(0.4),
    Dense(128, activation="relu"),
    Dropout(0.3),
    Dense(10, activation="softmax"),
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



## Installing and using R keras

This also takes some time to install and there is a risk that your session on the GPU node will time out. It's probably best to end the current session: type `exit`. This will bring you back to the login node. You might want to type `clear` to clear the screen as the prompt will often jump to somewhere randomly in the printed output. Start a new GPU session; 45 mins should be enough.

```bash
sinteractive --partition=atesting_a100 --time=0:45:00 --nodes=1 --ntasks=8 --gres=gpu:1 --qos=testing
```

Load anaconda

```bash
module load anaconda
```

Activate the conda environment

```bash
conda activate r-py-keras3
```

Start R by typing `R` at the prompt. First install the `codetools` library needed for compiling, then install the `keras` library. It will take some time to download, unpack, and compile all the libraries. Again, be patient. It might often seem like nothing is happening. This might take 20-30+ mins.

```
install.packages("codetools")
install.packages("keras3")
```

You will need to install any other R packages you want to use, such as `dplyr`, the same way.

Now we can use the R keras library. Work through this example to check that everything is working. This is the canonical keras example.

```r
# Tell reticulate which conda environment to use
reticulate::use_condaenv(condaenv="r-py-keras3")

# Check tensorflow GPU configuration. Should list and name all 
# GPU devices with status TRUE
tensorflow::tf_gpu_configured(verbose=TRUE)
tensorflow::tf$config$list_physical_devices("GPU")

# Now you can load keras (must be after `use_condaenv` above)
library(keras3)

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

# Fit should use GPU. It will take a few moments to set up the GPU the first time.
fit(model, x_train, y_train, epochs=30, batch_size=128, validation_split=0.2)

weights <- get_weights(model)
paste("Number of tensors:", length(weights)) #should be 6
```

When you have finished with R, type `q()` to quit R.  To end your session on the compute node, type `exit`. This will bring you back to the login node. You might want to type `clear` to clear the screen as the prompt will often jump to somewhere randomly in the printed output. To logout, type `logout`.
