# caffemodel-compression：

##  How it works:
       caffe源代码，只需要python接口，caffemodel和三个prototxt文件
       压缩率和具体数据集大小，以及背景复杂程度有关，举个例子：96.5MB的caffemodel(ssd model)压缩至580KB
## How to use：
       step(1) 先正常训练一个model，然后用python接口加载这个net，把每层的参数打印出来
       step(2) 训练一个小的网络，前3层滤波器个数和大网络一致，后面的几层使用精简参数
       step(3) 把精度高，体积大的网络的前三层，灌进小网络，然后savecaffemodel
       step(4) 然后 build/tools/caffe train -solver solver.prototxt -weights save.caffemodel 进行微调
## whats more：
       后面的几层可以随机初始化参数，也可以从大的model蒸馏
       把每个滤波器的权重求和，把大的滤波器留下，并灌到新的待灌model里（新model每层有多少滤波器，要由大model留下多少滤波器决定）
                                                  
