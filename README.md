# Realtime iOS Object Detection with TensorFlow

This Repository contains all the file to build a YOLO based object detection app except the tensorflow frozon model file, you can download the model file [here](http://www.google.com).

This app is dirivate from (Google's TensorFlow iOS Example)[]. Thanks to the [YOLO_tensorflow](https://github.com/gliese581gg/YOLO_tensorflow) project by gliese581gg, I take the tiny implementation and merge the proprocessing (resize the image and normalize each pixel) and result interpreting to the tensorflow graph, then froze the checkpoint data from glese581gg with the GraphDef to the pb file, and use it in the app.

## Build
- follow the instruction of the tensorflow buildin ios_example, to comile the protobuf and tensorflow core static library

- Clone this repository under the `tensorflow/contrib/ios_example` at same level of the offical camera project

- download the graph file and put it into data folder 

- now you can open the Xcode project file and compile, run it on your real device.

##Disclame

Despite I have already use YOLO tiny model, at runtime it still require at lease 850M memory, so only iPhone 6s or later which get at lease 2GB of memory can make it running, otherwise it will be killed immediately when loading the model.


##Froze the model by yourself
- clone the fork of [YOLO_tensorflow](https://github.com/yjmade/YOLO_tensorflow), download the weights checkpoint file provide by [gliese581gg](http://dropbox.com) and put it into the weights folder

- in ipython

```python
from YOLO_tiny_tf import YOLO_TF

yolo=YOLO_TF()
with open("weights/tiny_model.pb","wb") as f:
    f.write(yolo.sess.graph_def.SerializeToString())
```

- follow this [tutoral](https://tensorflow.org/) to build the tensorflow frozen tools

```bash
python -m tensorflow.python.tools.freeze_graph \
--input_graph=tiny_model.pb\ 
--input_checkpoint=YOLO_tiny.ckpt\
--output_graph=frozen_tiny.pb\
--output_node_names=classes_prob,classes_arg,boxes\ --input_binary=1
```

the output of frozen_tiny.pb then you can use it in the app.



