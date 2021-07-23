## Quick Start

1. Git clone yolovx project of u version and install requirements.txt

   ```
   git clone git@github.com:Megvii-BaseDetection/YOLOX.git
   cd YOLOX
   pip install -U pip && pip install -r requirements.txt
   pip install -v -e .
   git clone https://github.com/NVIDIA/apex
   cd apex
   pip install -v --disable-pip-version-check --no-cache-dir --global-option="--cpp_ext" --global-option="--cuda_ext" ./
   pip install cython
   pip install pycocotools
   pip install onnx-simplifier
   ```

2. To export onnx

   ```
   python tools/export_onnx.py -n yolox-s -c yolox-s.pth.tar
   ```

3. Onnx simplifer simplified model

   ```
   python -m onnxsim yolox-s.onnx yolox-s-sim.onnx
   ```

4. Generate ncnn param and bin file.

   ```
   cd <path of ncnn>
   cd build/tools/ncnn
   onnx2ncnn yolox-s-sim.onnx yolox-sim.param yolox-sim.bin
   ```

5. Since Focus module is not supported in ncnn. Warnings like:

   ```
   Unsupported slice step ! 
   Unsupported slice step ! 
   Unsupported slice step ! 
   ```

6. Reference resources:https://github.com/Outstanding/YoloV5s2ncnn

7. Use onnx_optimze to generate new param and bin:

   ```
   ncnnoptimize model.param model.bin yolox-sim.param yolox-sim.bin 65536
   ```

8.  Find /ncnn_root/examples/CMakeLists.txt add code:

   ```
   ncnn_add_example(yolox)
   ```

9. Recompile and run yolox. cpp

   ```text
   ncnn/build# make install
   ncnn/build/examples# ./yolox
   ```

## Thanks

https://github.com/Tencent/ncnn

https://github.com/Megvii-BaseDetection/YOLOX

