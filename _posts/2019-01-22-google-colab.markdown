---
title: "️👨‍💻 Quick guide to run your Python scripts on Google Colaboratory"
layout: post
date: 2019-01-22 23:00
published: true
headerImage: false
tag:
- Python
- DataScience
- Colab
category: blog
author: edenau
description: "Google Colaboratory"
# jemoji: '<img class="emoji" title=":open_file_folder:" alt=":open_file_folder:" src="https://assets.github.com/images/icons/emoji/unicode/1f5c2.png" height="20" width="20" align="absmiddle">'
---

# Introduction

If you are looking for an interactive way to run your Python script, say you want to start a machine learning project with a couple of friends, look no further — <a href="https://colab.research.google.com/" target="_blank">Google Colab</a> is the best solution for you. You can work online and save your code on your local Google Drive, and it allows you to

- Run your scripts with free GPUs (and TPUs!)
- Utilize pre-installed Python libraries and Jupyter Notebook features
- Work anywhere you want, it is on the clouds
- Share codes and collaborate with colleagues

In short,

>> Google Colab = Jupyter Notebook + Free GPUs

with arguably cleaner interface than most (if not all) alternatives. I have come up with some code snippets for you to master Google Colab. I hope this becomes a go-to article when you need some ready-made codes to solve common problems on Colab.


## Table of Contents
- [Basics](#basics)
- [Files](#files)
- [Libraries](#libraries)
- [Machine Learning](#ml)
- [Remarks](#remarks)

<div class="breaker"></div> <a id="basics"></a>

# Basics
## Enabling GPU/TPU Acceleration

Go to **‘Runtime’ > ‘Change runtime type’ > ‘Hardware accelerator’**, and select ‘GPU’ or ‘TPU’. You can check if the GPU is enabled by

```
import tensorflow as tf
device_name = tf.test.gpu_device_name()
if device_name != '/device:GPU:0':
  raise SystemError('GPU device not found')
```

An error will be raised if GPU is not enabled. Note that you can only run a session continuously for maximum 12 hours, and the environment do not persist across sessions.

## Running a Cell
***SHIFT + ENTER***

## Executing Bash Commands
Simply add an `!` before, for example:

```
!ls '/content/gdrive/My Drive/Colab Notebooks/'
```

Let’s check the information of OS, processors and RAM they are using:

```
!cat /proc/version
!cat /proc/cpuinfo
!cat /proc/meminfo
```

Linux, no surprise.

<div class="breaker"></div> <a id="files"></a>

# Files
## Accessing Files on Google Drive

Use the following script:

```
from google.colab import drive
drive.mount('/content/gdrive')
```

You will then be asked to sign in your Google account and copy an authorization code. ***Click the link, copy the code, paste the code.***

```
Go to this URL in a browser: https://accounts.google.com/signin/oauth/...
Enter your authorization code:
 ··········
Mounted at /content/gdrive
```

## Uploading Files

You can simply upload files manually to your Google Drive, and access them using codes above. Alternatively, you can use the following code:

```
from google.colab import files
uploaded = files.upload()
```

##Running Executable File

Copy the executable file to `/usr/local/bin`, and give yourself permission to execute it.

```
!cp /content/gdrive/My\ Drive/Colab\ Notebooks/<FILE> /usr/local/bin
!chmod 755 /usr/local/bin/<FILE>
```

<div class="breaker"></div> <a id="libraries"></a>

# Libraries
## Installing Libraries

Use `pip` in bash command:

```
!pip install <PACKAGE_NAME>
```

or `conda`:

```
!wget -c https://repo.continuum.io/archive/Anaconda3-5.1.0-Linux-x86_64.sh
!chmod +x Anaconda3-5.1.0-Linux-x86_64.sh
!bash ./Anaconda3-5.1.0-Linux-x86_64.sh -b -f -p /usr/local
!conda install -q -y --prefix /usr/local -c conda-forge <PACKAGE_NAME>
import sys
sys.path.append('/usr/local/lib/python3.6/site-packages/')
```

<div class="breaker"></div> <a id="ml"></a>

# Machine Learning
## Tensorboard

Use `ngrok`:

```
# Run Tensorboard in the background
LOGDIR = '/tmp/log'
get_ipython().system_raw(
    'tensorboard --logdir {} --host 0.0.0.0 --port 6006 &'
    .format(LOGDIR)
)
# Use ngrok to tunnel traffic to localhost
! wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
! unzip ngrok-stable-linux-amd64.zip
get_ipython().system_raw('./ngrok http 6006 &')
# Retrieve public url
! curl -s http://localhost:4040/api/tunnels | python3 -c \
    "import sys, json; print(json.load(sys.stdin)['tunnels'][0]['public_url'])"
```

and you will get a hyperlink like this:

```
https://d5b842b9.ngrok.io
```

You can access your Tensorboard via the link!

<div class="breaker"></div> <a id="remarks"></a>

# Remarks

Thank you for reading. I will try to keep this article up-to-date. Hope this helps!
