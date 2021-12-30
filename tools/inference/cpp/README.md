<h1 align="center">PaddleRec inference C++（线上）</h1>
本目录是 paddlerec C++ 线上预测完整 demo 工程，包括 debug、基于 cube 的 kv 查询等功能，适用 os 为 linux：

<h2>paddlerec 中引入 cube 的背景</h2>

成熟的推荐系统的排序算法通常都有规模相当大的 embedding 表，这些 embedding 表在单台机器上存放不下，因此通常的做法是 embedding 拆解存放在外部 KV 服务上。预测时，大规模稀疏参数不需要存放在本地，而是直接去远端查询，避免了本地 lookup_table 操作。

<h2>代码目录</h2>

```
tools/inference/cpp         
|-- main.cpp # 工程 main 文件    
|-- src   # cpp 文件  
|-- include   # h 文件  
|-- proto   # proto 文件及对应的 pb 文件  
|-- data  # 输入样例数据  
|-- ref_docs   # 相关使用文档
|-- keys   # kv 功能样例 key 文件  
|-- user.flags   # 命令行参数配置文件  
|-- run.sh  # 启动脚本
|-- 其他

```

<h2>准备工作</h2>

1. PaddlePaddle C++ 预测库及第三方依赖库：[下载地址](https://paddlerec.bj.bcebos.com/online_infer/paddle_inference.tar.gz)
2. 模型和参数文件：[地址1](https://paddlerec.bj.bcebos.com/online_infer/all_model_params.tar.gz)、[地址2](https://paddlerec.bj.bcebos.com/online_infer/new_model.tar.gz)、[地址3](https://paddlerec.bj.bcebos.com/online_infer/small_model.tar.gz)
3. cube 使用样例：[节点内配送](https://paddlerec.bj.bcebos.com/online_infer/cube_app.tar.gz)、[跨节点配送](https://paddlerec.bj.bcebos.com/online_infer/cube_app2.tar.gz)

以上文件下载后，直接解压到当前目录。

<h2>cube kv 功能流程</h2>

* 请阅读 ref_docs 目录下的文件

<h2>测试用例</h2>

1. 不启用 cube 服务：全量带 embedding 的模型

2. 启用 cube 服务：比如裁剪后不带 embedding 的模型（small_model, 300 个 slot）、输入数据（demo_10_300）、可打印的 varname（all_vars_small_model.txt）

|模型|样本文件|slot个数 |用户可打印的 var_names|
|-|-|--|-|
|all_model_params/model_params（没裁剪）| demo_10_uint32 | 408 | all_vars2.txt |
|new_model（裁剪后）|demo_10_408|408|all_vars.txt|
|small_model（裁剪后）|demo_10_300|300|all_vars_small_model.txt|  
  

<h2>编译运行</h2>

我们准备了 run.sh 脚本提供一键编译并运行，主要启动参数如下：
|        名称         |    类型    |             取值             | 是否必须 |                               作用描述                               |
| :-----------------: | :-------: | :--------------------------: | :-----: | :------------------------------------------------------------------: |
|       --modelFile        |    string    |       任意路径         |    是    |                            模型文件                           |
|       --paramFile        |    string    |       任意路径         |    是    |                            参数文件                           |
|       --trainingFile        |    string    |       任意路径         |    是    |                            样例输入数据                           |
|       --debug        |    bool    |       true/false         |    否    |                            是否使用debug功能                            |
|       --threadNum        |    int32    |       >=1         |    是    |                            预测线程个数                            |
|       --batchSize        |    int32    |       >= 1         |    是    |                            批预测样本数量                            |
|       --withCube        |    bool    |       true/false         |    否    |                            测试是否使用 cube 功能                            |
|       --cube_batch_size        |    uint64    |       >=1         |    否    |                            cube 批量查询大小                            |
|       --cube_thread_num        |    int32    |       >=1         |    否    |                            cube 查询线程数                            |
|       --config_file        |    string    |       任意路径         |    否    |                            cube client 端需要感知的 cube server 配置参数                            |
|       DWITH_CUBE        |    cmake编译选项    |       ON/OFF         |    否    |                            和 --withCube 一起使用，需要修改对应的模型和参数文件                            |
|       DWITH_CUBE_CONVERT2        |    cmake编译选项    |       ON/OFF         |    否    |                            和 --withCube 一起使用，cube文件格式转换方法                            |

<h2>注：</h2>
 
* 样例中的模型文件由 PaddlePaddle develop 分支生成的  
* paddle inference 预测库默认版本 2.1   
* cube client 代码基于 Paddle Serving v0.3.0 server  
* cube-builder、cube、cube-transfer、cube-agent、brpc 由 Paddle Serving v0.6.2 server 编译出来的，当然 v0.3.0 也可以编出来
* 由于该项目能加载完整模型和裁剪后的模型，测试时注意修改模型文件和数据路径（在 user.flags 文件中）和 run.sh 中 cmake 编译命令
