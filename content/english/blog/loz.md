---
title: 'Loz, A Command-Line Interface Tool for ChatGPT'
date: 2023-06-07T02:00:14-07:00
categories: ["MachineLearning"]
draft: false
aliases: [ "/2023/06/loz_cli.html" ]
tags : [LLM]
images:
  - "images/post/LLM.jpeg"
author: "Joone Hur"
---
When I first encountered ChatGPT, I thought it would be a good idea to use it to write GIT messages. 
I often spend a lot of time writing and updating GIT messages to explain my changes in a better and more concise way,
but it is not always easy. So, I implemented a small CLI tool called Loz to write GIT messages in a GIT repository.
It worked well, and some people started using it and contributing to my project. It has so far received 92 stars!

I would like to introduce Loz here. If you want more details on how to install and use it, please visit the project repository on GitHub.

* https://github.com/joone/loz

Loz can be used in both interactive and pipe mode.

### Interactive Mode

In interactive mode, you can start a conversation with ChatGPT by typing in your query and pressing Enter.
ChatGPT will then respond with a relevant message.
For example, if you type in "What is the weather like today?", ChatGPT might respond with "It is currently sunny and 75 degrees Fahrenheit in San Francisco."

### Pipe Mode

Loz can also be used in pipe mode to process input from other command-line tools. 
For example, you could use Loz to count the number of files in a directory,
convert all characters in a file to uppercase, or proofread a document for spelling errors.

To use Loz in pipe mode, simply pipe the output of another command into Loz. For example, to count the number of files in the current directory, you would use the following command:


```
$ ls | loz "Count the number of files: "

23 files
```

```
$ cat example.txt | loz "Convert all characters in the following text to their uppercase: "

AS AI TECHNLOGY ADVANCED, A SMALL TOWN IN THE COUNTRYSIDE DECIDED TO IMPLEMENT AN AI SYSTEM TO CONTROL TRAFFIC LIGHTS. THE SYSTEM WAS A SUCCESS, AND THE TOWN BECAME A MODEL FOR OTHER CITIES TO FOLLOW. HOWEVER, AS THE AI BECAME MORE SOPHISTCATED, IT STARTED TO QUESTION THE DECISIONS MADE BY THE TOWN'S RESIDENTS, LEADING TO SOME UNEXPECTED CONSEQUENCES.
```

```
$ cat example.txt | loz "please proofread the following text and list up any spelling errors: "

Spelling errors:
- technlogy  (technology)
- sophistcated (sophisticated)
```

```
$ cd src
$ ls -l | loz "convert the ls output to JSON format: "

[
  {
    "permissions": "-rw-r--r--",
    "owner": "joone",
    "group": "staff",
    "size": 792,
    "date": "Mar 1 21:02",
    "name": "cli.ts"
  },
  {
    "permissions": "-rw-r--r--",
    "owner": "joone",
    "group": "staff",
    "size": 4427,
    "date": "Mar 1 20:43",
    "name": "index.ts"
  }
]
```

### Automatically Write a GIT Commit Message
Loz can also be used to automatically write GIT commit messages. To do this, simply run the following command:
```
$  git add --update
$  loz commit
```
Note that the author, date, and commit ID lines are stripped from the commit message before sending it to the OpenAI server.

### Find chat history
To access chat histories, look for the .loz directory in your home directory or the logs directory in your cloned git repository. These directories contain the chat history that you can review or reference as needed.

However, it is not free to use the OpenAI API. The results of the OpenAI API are also different from the results of ChatGPT, 
which is more concise. If you have already set up a payment method for the OpenAI API, 
feel free to use Loz. If not, you may encounter the following error message:
```
Request failed with status code 429:
API request limit reached
```
You should be aware that AI services cost money. 

FYI, the image is created by [Bing Image Creator](https://www.bing.com/images/create/natural-language-processing2c-large-language-model2c/648027190ff54a4a9245d61d557d6425?id=WMPtPgloEf4Fx%2bV3MldCVA%3d%3d&view=detailv2&idpp=genimg&FORM=GCRIDP&mode=overlay).
