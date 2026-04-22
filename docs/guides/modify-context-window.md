# Modifying the context window of a model

By default, Ollama uses a context window based on the available VRAM
- Less than 24 GB VRAM results in a 4K context window
- 24 to 48 GB VRAM results in a 32K context window
- 48 GB or more is 256K in context window

There are two ways to increase the context window of the model.

1. Creating a Modelfile for a model that is not yet installed
2. Modifying an already installed model

## Modelfile

To increase the context window with a Modelfile you can do the following. Using a Modelfile you don't need to have the model installed previously as this process will also download it automatically

```bash
touch Modelfile
nano Modelfile
```

Inside the model file add the following

```bash
FROM <YOUR_MODEL>
PARAMETER num_ctx 32768 # I will be using a window of 32K tokens
```

Lastly run the following command to download the model with 32K Context using Ollama run

```bash
ollama create <YOUR_MODEL>-32k -f Modelfile
```

## Modifying an already installed model

If you already have a model installed you can modify it to have 32k context doing the following

> [!NOTE]
> This will still create a new model on your computer

```bash
ollama run <YOUR_MODEL>
>>> /set parameter num_ctx 32768
>>> /save <YOUR_MODEL>-32k
>>> /bye
```

You should be able to check the available modes on your computer by using the following command

```bash
ollama list
```
In this guide you will learn how to setup you Mac computer as a LLM Server and access your models from a Windows machine using OpenCode.

If you encounter any errors or wrong information on this guide create an Issue and I'll take a look at it.

You should see `<YOUR_MODEL>-32k` in the list 