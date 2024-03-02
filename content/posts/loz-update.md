---
title: "Loz Update: Now with Local LLM Support and Natural Language Linux Commands"
date: 2024-02-26T19:44:48-08:00
draft: false
categories: ["AI"]
tags : [MyProject, LLM]
---

![alt Loz Demo](https://github.com/joone/loz/raw/main/examples/loz_demo.gif?raw=true)

I've been developing the Loz project in my spare time for nearly a year. It's now ready for others to use, so I'm excited to share the latest updates.

https://github.com/joone/loz


## Support for Local LLM
Initially, Loz only supported the OpenAI API. A friend inquired about integrating open-source LLMs like Llama2, which isn't straightforward to set up on a user's computer. I discovered a project called [Ollama](https://github.com/ollama/ollama) that simplifies the installation of various open-source LLMs with a single command. It supports GPU or CPU and is compatible with Linux, MacOS, and Windows, making it an ideal solution for Loz. I refactored Loz's codebase, introduced an abstract class named LLMService, and added subclasses such as OpenAiAPI and OllamaAPI. Now, users can effortlessly switch between LLMs in interactive mode using `config api` command.

An advantage is the ability to compare outputs across different LLM engines. I've tested Llama2, codellama, and GPT-3 for generating Git messages. llama2 tends to be unstable with long token sizes, producing verbose and inconsistent outputs. Sometimes, it even includes code diffs in the generated commit messages, indicating that Llama2 needs a finue-tune for this task.

## Execute Linux Commands Using Natural Language
I've added functionality to execute Linux commands via natural language. Loz first determines if the prompt is for generating a Linux command. If so, it returns the command(s) in JSON format, facilitating easy access to the command for Loz. If not, the prompt is forwared to OpenAI API and processed as a regular prompt.

The generated JSON varies but is generally understandable, containing command or commands objects and args object. Loz interprets these objects to construct executable Linux commands. However, there are challenges, such as when the tools referenced by the command are not installed on the local system. While enhancing the detail in prompts can improve the output, the results may still be unpredictable. Therefore, the continuous refinement of Git message and Linux command generation remains an ongoing effort.

Here are some examples of executing Linux commands with Loz:

### Find the largest file in the current directory:
  ```
  loz "find the largest file in the current directory"
  -rw-rw-r-- 1 foo bar 9020257 Jan 31 19:49 ./node_modules/typescript/lib/typescript.js
  ```

### Check if Apache2 is running:
  ```
  loz "check if apache2 is running on this system"
  ● apache2.service - The Apache HTTP Server
  ```

### Detect GPUs in the system:
  ```
  loz "Detect GPUs on this system"
  00:02.0 VGA compatible controller: Intel Corporation Device a780 (rev 04)
  ```

## CLI for LLM
In the future, familiarity with command-line tools for Linux, Windows, or MacOS might become unnecessary, as we will interact directly with a built-in LLM within the operating system. Loz serves as a precursor to this envisioned future.
