
# Setp1：

* 生成分片文件

    ```
    ./cube_app/cube-builder -dict_name=test_dict -job_mode=base -last_version=0 -cur_version=0 -depend_version=0 -input_path=./cube_app/cube_model -output_path=${PWD}/cube_app/data -shard_num=1 -only_build=false  
    ```

此处可能需要用户升级 libcurl(apt-get install libcurl3)

# Step2：

* 启动 cube server

    ```
    mv ./cube_app/data/0_0/test_dict_part0/* ./cube_app/data/
    
    cd cube_app && ./cube 
    ```

# Step3：

* 用户验证

    ```
    ./cube-cli -dict_name=test_dict -keys [keys文件] -conf ./cube_app/cube.conf # 本项目中 cube client 的功能已经集成在 inference 代码里了
    ```  