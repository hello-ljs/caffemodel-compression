# caffemodel-compression
##现有的model压缩方法总结：
    ###1：只有caffe框架下可以压缩
    ###2：压缩都会有精度损失，深鉴也不例外
    ###3：目前最知名的压缩方法为深鉴科技的deep compression ，这个方法需要修改caffe源代码中blob的存储方式，需要用到稀疏矩阵
       修改起来难度很大，并且，deep compression的方法（剪枝，量化，哈夫曼编码）并不适合适用，原因在于该方法为深鉴科技为其硬件垂直设计的，
       也就是说，只有在深鉴的开发板上，可以适用该方法。
       深鉴科技神经网络的压缩仅支持CNN网络的卷积层和全链接层
       深鉴科技压缩收费50万人民币。
    ###4：我的方法不需要修改caffe源代码，只需要python接口，caffemodel和三个prototxt文件
       压缩率和具体数据集大小，以及背景复杂程度有关，举个例子：96.5MB的caffemodel(ssd model)压缩至580KB
##具体使用方法：
  ###先正常训练一个model，然后用python接口加载这个net，把每层的参数打印出来
  ###训练一个小的网络，前3层滤波器个数和大网络一致，后面的几层使用精简参数
  ###把精度高，体积大的网络的前三层，灌进小网络，然后savecaffemodel
  ###然后 build/tools/caffe train -solver solver.prototxt -weights save.caffemodel
##另外：
    ###后面的几层可以随机初始化参数，也可以从大的model蒸馏
    ###把每个滤波器的权重求和，把大的滤波器留下，并灌到新的待灌model里（新model每层有多少滤波器，要由大model留下多少滤波器决定）
                                                  
