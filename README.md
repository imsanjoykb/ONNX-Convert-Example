# onnx-convert-example

Simple example how to convert an PyTorch model into Tensorflow using ONNX

Adding packages if you don't have them, if you don't need to move files to the current installation you will not need the pure-ftpd package.
```
apt install python3 python3-pip git pure-ftpd
```

Install python dependencies in order to run the convert script.
```
pip3 install torchvision
pip3 install onnx
pip3 install tensorflow
pip3 install onnx_tf
```

## Build an engine from onnx using trtexec tools

```shell
trtexec --verbose \
        --nvtxMode=verbose \
        --buildOnly \
        --workspace=8192 \
        --onnx=your_onnx.onnx \
        --saveEngine=your_engine.engine \
        --timingCacheFile=timing.cache \
        --fp16 # use fp16
```

Notice: `--nvtxMode=verbose` is the same as `--profilingVerbosity=detailed`

You will get a `your_engine.engine` and a `timing.cache`

## Parser network from engine using trtexec tools

```shell
trtexec --verbose \
        --noDataTransfers \
        --useCudaGraph \
        --separateProfileRun \
        --useSpinWait \
        --nvtxMode=verbose \
        --loadEngine=your_engine.engine \
        --exportLayerInfo=graph.json \
        --timingCacheFile=timing.cache
```

Running the script 
```
python3 pytorch_to_tensorflow_lite.py models/mnist.pth models
```
