环境准备
激活 conda 环境

```
conda activate dbgpt
```

切换到 DB-GPT 目录

```
cd /root/DB-GPT/
```

导入 SQLite 样例数据

导入 SQLite 样例数据

切换到 DB-GPT 目录

```
cd /root/DB-GPT/
```

导入 SQLite 样例数据

导入 SQLite 样例数据

```
bash ./scripts/examples/load_examples.sh
```

运行 DB-GPT
使用命令行工具启动

```
dbgpt start webserver --port 6006
```

使用其他模型
你可以修改 /root/DB-GPT/.env 的配置来修改其它的配置。

AutoDL 中默认的系统盘空间比较小，你可以将模型下载到数据盘目录 /root/autodl-tmp/ 下，然后创建一个模型的软连接。

下面的步骤将示范如何使用 vicuna-13b-v1.5 模型：

切换工作路径到数据盘目录

```
cd /root/autodl-tmp/
```


下载模型文件

```
git clone https://huggingface.co/lmsys/vicuna-13b-v1.5
```


创建软连接

```
ln -snf /root/autodl-tmp/vicuna-13b-v1.5   /root/DB-GPT/models/vicuna-13b-v1.5
```


将 /root/DB-GPT/.env 文件中的 LLM_MODEL 配置从 chatglm2-6b 修改为 vicuna-13b-v1.5

```
sed -i 's/LLM_MODEL=chatglm2-6b/LLM_MODEL=vicuna-13b-v1.5/' /root/DB-GPT/.env
```


修改后如图：


重新启动 DB-GPT

```
cd /root/DB-GPT/
dbgpt start webserver --port 6006


```

