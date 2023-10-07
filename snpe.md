
# download snpe

[https://developer.qualcomm.com/qfile/68747/snpe-1.53.2.zip](https://developer.qualcomm.com/qfile/68747/snpe-1.53.2.zip)

zip snpe-1.53.2.zip

# run docker

```
docker pull jungin500/snpe:1.5x

cd snpe-1.53.2.2811

docker run -it --rm -v .:/snpe jungin500/snpe:1.5x
```
# convert onnx to dlc

add snpe lib to PYTHONPATH

add LD_LIBRARY_PATH

```
export PYTHONPATH=${SNPE_ROOT}/lib/python:${PYTHONPATH}
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/snpe/lib/x86_64-linux-clang
```

convert model
```
cd /snpe/bin/x86_64-linux-clang

./snpe-onnx-to-dlc -i model-hao.onnx
```