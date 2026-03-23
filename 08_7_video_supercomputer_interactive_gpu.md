# Interactive mode on supercomputer

To fit a CNN on an Nvidia A100 GPU

See **Week 8 Using the supercomputer - interactive.mp4** in the online lecture recordings.

The video is from last year so there are a few minor differences.

1) You must now include a quality of service (qos) setting to request a node.

```bash
sinteractive --partition=atesting_a100 --time=0:60:00 --nodes=1 --ntasks=8 --gres=gpu:1 --qos=testing
```

2) The video uses keras 2. Keras 3 works the same except for setting up a conda environment to use keras 3 (now `r-py-keras3`, see CU_supercomputer_keras3_gpu.md), and some other minor differences in the updated code.
