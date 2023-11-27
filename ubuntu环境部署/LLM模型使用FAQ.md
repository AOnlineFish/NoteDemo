#  **bash** LLM模型使用FAQ

## 我怎么使用OPENAI服务

通过在.env文件设置LLM_MODEL

```bash
LLM_MODEL=proxyllm
```



set your OPENAPI KEY

```bash
PROXY_API_KEY={your-openai-sk}
PROXY_SERVER_URL=https://api.openai.com/v1/chat/completions
```



确认openapi API_KEY是否可用

## Q2 `python dbgpt_server --light` 和 `python dbgpt_server`的区别是什么?

备注

- `python dbgpt_server --light` dbgpt_server在启动后台服务的时候不启动模型服务, 用户可以通过`python llmserver`单独部署模型服务，dbgpt_server通过LLM_SERVER环境变量来访问模型服务。目的是为了可以将dbgpt后台服务和大模型服务分离部署。
- `python dbgpt_server` 是将后台服务和模型服务部署在同一台实例上.dbgpt_server在启动服务的时候同时开启模型服务.

## Q3 how to use MultiGPUs

DB-GPT默认加载可利用的gpu，你也可以通过修改 在`.env`文件 `CUDA_VISIBLE_DEVICES=0,1`来指定gpu IDs

你也可以指定gpu ID启动

```bash
# Specify 1 gpu
CUDA_VISIBLE_DEVICES=0 python3 pilot/server/dbgpt_server.py

# Specify 4 gpus
CUDA_VISIBLE_DEVICES=3,4,5,6 python3 pilot/server/dbgpt_server.py
```



同时你可以通过在.env文件设置`MAX_GPU_MEMORY=xxGib`修改每个GPU的最大使用内存

## Q4 Not Enough Memory

DB-GPT 支持 8-bit quantization 和 4-bit quantization.

你可以通过在.env文件设置`QUANTIZE_8bit=True` or `QUANTIZE_4bit=True`

Llama-2-70b with 8-bit quantization 可以运行在 80 GB VRAM机器， 4-bit quantization可以运行在 48 GB VRAM

Note: you need to install the latest dependencies according to [requirements.txt](https://github.com/eosphoros-ai/DB-GPT/blob/main/requirements.txt).

## Q5 How to Add LLM Service dynamic local mode

DB-GPT支持多个模型服务切换, 怎样添加一个模型服务呢

```bash
dbgpt model start --model_name ${your_model_name} --model_path ${your_model_path}

chatglm2-6b
eg: dbgpt model start --model_name chatglm2-6b --model_path /root/DB-GPT/models/chatglm2-6b

chatgpt
eg: dbgpt model start --model_name chatgpt_proxyllm --model_path chatgpt_proxyllm --proxy_api_key ${OPENAI_KEY} --proxy_server_url {OPENAI_URL}
```



## Q6 How to Add LLM Service dynamic in remote mode

如果你想在远程机器实例部署大模型服务并添加到本地dbgpt_server进行管理

使用1`dbgpt start worker`命令并设置注册地址–controller_addr

```bash
eg: dbgpt start worker --model_name vicuna-13b-v1.5 \
--model_path /app/models/vicuna-13b-v1.5 \
--port 8002 \
--controller_addr http://127.0.0.1:8000
```



## Q7 dbgpt command not found

```bash
pip install -e "pip install -e ".[default]"
```



## 云服务器启动worker_manager注册到controller时，发现worker暴露的ip是私网ip, 没有以公网ip暴露，导致服务访问不到

```bash
--worker_register_host  public_ip     The ip address of current worker to register
                                to ModelController. If None, the address is
                                  automatically determined
```