# DBNet

The Pytorch implementation is [DBNet](https://github.com/BaofengZan/DBNet.pytorch).

## How to Run

本仓库提供的CmakeLists.txt为win版本。Linux版本请看最后的链接。

* 1 生成wts
  从pytoch仓库下载到代码和模型。配置好环境后

  在tools/predict.py中，将save_wts属性置为True，运行后，就在在tools文件夹下生成wts文件。

  同时也可以导出onnx。将onnx属性设置为True即可。

* 2 cmake 生成工程

  ```
  mkdir build
  cd build
  cmake ..
  make
  ./dbnet -s             // serialize model to plan file i.e. 'DBNet.engine'
  ./dbnet -d  ../samples // deserialize plan file and run inference, the images in samples will be processed.
  ```

  ![image-20200807183412846](https://user-images.githubusercontent.com/20653176/89722330-00c36900-da1b-11ea-97f4-c61f9cd196fa.png)

## linux版本

https://github.com/wang-xinyu/tensorrtx



## 不足

* 1 common文件中，下面两个函数可以合并，自己偷了个懒。

```c++
ILayer* convBnLeaky(INetworkDefinition *network, std::map<std::string, Weights>& weightMap, ITensor& input, int outch, int ksize, int s, int g, std::string lname, bool bias = true) 
```

```c++
ILayer* convBnLeaky2(INetworkDefinition *network, std::map<std::string, Weights>& weightMap, ITensor& input, int outch, int ksize, int s, int g, std::string lname, bool bias = true)
```

* 2 后处理中与pytorch版本也有好多不同之处，这都是可以改进提升的。
* 3 在pyorch中数据预处理是将图像短边resize到1024，长边按比例缩放，最后将新的宽高截到32的倍数。而在自己的repo中直接将图像resize到640*640，较为粗暴。