
# Step1：

* 将明文文件 file.txt 放在 cube_app2 目录下

    ```
    cd cube_app2
    ```

* 执行 python 脚本，明文转二进制文件

    ```
    cat file.txt | python kv_to_seqfile.py  # 当前目录
    ```

# Step2：

* 启动cube&cube-agent

* 进入 cube_app2/cube 目录

    ```
    nohup bash start.sh &
    ```

# Step3：

* 进入 cube_app2/transfer 目录

    ```
    ./cube-transfer -p 8099 -l 4 --config conf/transfer.conf
    ```

* 最后出现 wait 5 min字样的时候 可以Ctrl+C退出

# Step4：

* cube-cli验证配送， 验证key在keys_10 文件

    ```
    cd cube_app2/cube-cli
    sh seek.sh
    ```

