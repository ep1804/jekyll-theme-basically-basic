---
layout: post
title:  Conda install and create python 3 and 2 environment
description: "Conda install and setup env"
modified: 2019-10-26
tags: [conda]
---

Conda install and setup python 3 and 2 environment

{% raw %}

ref: https://docs.conda.io/en/latest/miniconda.html#installing


### download and install Miniconda

```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
bash Miniconda3-latest-Linux-x86_64.sh
```

### create python 3.6 environment

```bash
conda create --name py3 python=3.6
```

### create python 2 environment

```bash
conda create --name py2 python=2
```

### activate python 3 environment

```bash
conda activate py3
```

{% endraw %}
