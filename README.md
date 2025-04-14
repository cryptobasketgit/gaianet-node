# Run your own GaiaNet node


<p align="center">
  <a href="https://discord.gg/gaianet-ai">
    <img src="https://img.shields.io/badge/chat-Discord-7289DA?logo=discord" alt="GaiaNet Discord">
  </a>
  <a href="https://twitter.com/Gaianet_AI">
    <img src="https://img.shields.io/badge/Twitter-1DA1F2?logo=twitter&amp;logoColor=white" alt="GaiaNet Twitter">
  </a>
   <a href="https://www.gaianet.ai/">
    <img src="https://img.shields.io/website?up_message=Website&url=https://www.gaianet.ai/" alt="Gaianet website">
  </a>
</p>

[Japanese(日本語)](README-ja.md) | [Chinese(中文)](README-cn.md) | [Korean(한국어)](README-kr.md) | [Turkish (Türkçe)](README-tr.md) | [Farsi(فارسی)](README-fa.md) | [Arabic (العربية)](README-ar.md) | [Indonesia](README-id.md) | [Russian (русскийة)](README-ru.md) | [Portuguese (português)](README-pt.md) | We need your help to translate this README into your native language.

Like our work? ⭐ Star us!

Checkout our [official docs](https://docs.gaianet.ai/) and a [Manning ebook](https://www.manning.com/liveprojectseries/open-source-llms-on-your-own-computer) on how to customize open source models.

---

## Quick start

Install the default node software stack with a single line of command on Mac, Linux, or Windows WSL.

```bash
curl -s https://raw.githubusercontent.com/ankein228/gaianet-node/main/install.sh | bash
```

> Then, follow the prompt on your screen to set up the environment path. The command line will begin with `source`.

![image](https://github.com/user-attachments/assets/dc75817c-9a54-4994-ab90-1efb1a018b17)


Initialize the node. It will download the model files and vector database files specified in the `$HOME/gaianet/config.json` file, and it could take a few minutes since the files are large.

```bash
gaianet init
```

Start the node.

```bash
gaianet start
```

The script prints the official node address on the console as follows.
You can open a browser to that URL to see the node information and then chat with the AI agent on the node.

```
... ... https://0xf63939431ee11267f4855a166e11cc44d24960c0.us.gaianet.network
```

To stop the node, you can run the following script.

```bash
gaianet stop
```

## Install guide

```bash
curl -s https://raw.githubusercontent.com/ankein228/gaianet-node/main/install.sh | bash
```

<details><summary> The output should look like below: </summary>

```console
[+] Downloading default config file ...

[+] Downloading nodeid.json ...

[+] Installing WasmEdge with wasi-nn_ggml plugin ...

Info: Detected Linux-x86_64

Info: WasmEdge Installation at /home/azureuser/.wasmedge

Info: Fetching WasmEdge-0.13.5

/tmp/wasmedge.2884467 ~/gaianet
######################################################################## 100.0%
~/gaianet
Info: Fetching WasmEdge-GGML-Plugin

Info: Detected CUDA version:

/tmp/wasmedge.2884467 ~/gaianet
######################################################################## 100.0%
~/gaianet
Installation of wasmedge-0.13.5 successful
WasmEdge binaries accessible

    The WasmEdge Runtime wasmedge version 0.13.5 is installed in /home/azureuser/.wasmedge/bin/wasmedge.


[+] Installing Qdrant binary...
    * Download Qdrant binary
################################################################################################## 100.0%

    * Initialize Qdrant directory

[+] Downloading the rag-api-server.wasm ...
################################################################################################## 100.0%

[+] Downloading dashboard ...
################################################################################################## 100.0%
```

</details>

By default, it installs into the `$HOME/gaianet` directory. You can also choose to install into an alternative directory.

```bash
curl -sSfL 'https://raw.githubusercontent.com/GaiaNet-AI/gaianet-node/main/install.sh' | bash -s -- --base $HOME/gaianet.alt
```

## Initialize the node

```
gaianet init
```

<details><summary> The output should look like below: </summary>

```bash
[+] Downloading Llama-2-7b-chat-hf-Q5_K_M.gguf ...
############################################################################################################################## 100.0%############################################################################################################################## 100.0%

[+] Downloading all-MiniLM-L6-v2-ggml-model-f16.gguf ...

############################################################################################################################## 100.0%############################################################################################################################## 100.0%

[+] Creating 'default' collection in the Qdrant instance ...

    * Start a Qdrant instance ...

    * Remove the existed 'default' Qdrant collection ...

    * Download Qdrant collection snapshot ...
############################################################################################################################## 100.0%############################################################################################################################## 100.0%

    * Import the Qdrant collection snapshot ...

    * Recovery is done successfully
```

</details>

The `init` command initializes the node according to the `$HOME/gaianet/config.json` file. You can use some of our pre-set configurations. For example, the command below initializes a node with the llama-3 8B model with a London guidebook as knowledge base.

```bash
gaianet init --config https://raw.githubusercontent.com/GaiaNet-AI/node-configs/main/llama-3-8b-instruct_london/config.json
```

To see a list of pre-set configurations, you can do `gaianet init --help`.
Besides a pre-set configurations like `gaianet_docs`, you can also pass a URL to your own `config.json` for the node to be initialized to the state you'd like.

If you need to `init` a node installed in an alternative directory, do this.

```bash
gaianet init --base $HOME/gaianet.alt
```

## Start the node

```
gaianet start
```

<details><summary> The output should look like below: </summary>

```bash
[+] Starting Qdrant instance ...

    Qdrant instance started with pid: 39762

[+] Starting LlamaEdge API Server ...

    Run the following command to start the LlamaEdge API Server:

wasmedge --dir .:./dashboard --nn-preload default:GGML:AUTO:Llama-2-7b-chat-hf-Q5_K_M.gguf --nn-preload embedding:GGML:AUTO:all-MiniLM-L6-v2-ggml-model-f16.gguf rag-api-server.wasm --model-name Llama-2-7b-chat-hf-Q5_K_M,all-MiniLM-L6-v2-ggml-model-f16 --ctx-size 4096,384 --prompt-template llama-2-chat --qdrant-collection-name default --web-ui ./ --socket-addr 0.0.0.0:8080 --log-prompts --log-stat --rag-prompt "Use the following pieces of context to answer the user's question.\nIf you don't know the answer, just say that you don't know, don't try to make up an answer.\n----------------\n"


    LlamaEdge API Server started with pid: 39796
```

</details>

You can start the node for local use. It will be only accessible via `localhost` and not available on any of the GaiaNet domain's public URLs.

```bash
gaianet start --local-only
```

You can also start a node installed in an alternative base directory.

```bash
gaianet start --base $HOME/gaianet.alt
```

### Stop the node

```bash
gaianet stop
```

<details><summary> The output should look like below: </summary>

```bash
[+] Stopping WasmEdge, Qdrant and frpc ...
```

</details>

Stop a node installed in an alternative base directory.

```bash
gaianet stop --base $HOME/gaianet.alt
```
### Chat with Your Node
To interact with your node and earn XP, visit: [Megaeth Gaia Domain](https://megaeth.gaia.domains)

* You need **Credit Balance** to be able to chat with your node.
* Everyday, Convert your **Gaiapoints** to **Credit Balance**.
* Your Gaiapoints will remain same after converting and You can convert 1000 gaiapoints everyday.

![image](https://github.com/user-attachments/assets/ba7e9d4c-70b7-4621-97ae-7f0633303154)


---

### Run AutoChat Bot 
1. **Create an API Key**:
   - Go to [Gaia API Keys](https://www.gaianet.ai/setting/gaia-api-keys) and create a new key. Save the API key somewhere since you won't be able to access it again.

2. **Download the Python Script**:
   - Run:
     ```bash
     curl -L -o gaiabot.py https://github.com/cryptobasketgit/gaianet-node/blob/main/gaiabot.py
     ```

3. **Run the Script**:
   - Open a screen to run the bot in the background, so if you closed the terminal, it won't stop:
     ```bash
     screen -S gaiabot
     ```
   - Now run this command to start the bot:
     ```bash
     python3 gaiabot.py
     ```
   - Enter your Gaia API key when prompted.

* To minmize the screen, press `Ctrl+A+D`
* To return the screen, enter command: `screen -r gaiabot`
* To stop and kill the bot, press `CTRL+C` inside the screen & run this command: `screen -XS gaiabot quit`

It might gives you failed attempts which is because of networks floods, as you can see in the picture that I may get a successfull response after 3 or more attempts

![image](https://github.com/user-attachments/assets/71ce30c6-2c3d-44b5-a3f5-b2a7781062bb)
