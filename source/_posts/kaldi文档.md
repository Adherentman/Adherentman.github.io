---
title: Kaldi语音识别初用
date: 2018-02-04 14：52
comments: true
layout: post
tags: [机器学习]
categories: 机器学习
---
# Kaldi启用

## INSTALL

1. 在 tools/ 下跑 extras/check_dependencies.sh
    1. 然后跑make
2. 之后在 src/ 下跑 ./configure --shared 在跑这句命令之前一定要先执行第一条
    1. make depend
    2. make
<!--more-->
### 细节

#### 修改路径

设置n = 4

在kaldi/egs 下跑 `vi thchs30/s5/run.sh` 


`thchs=/home/xuzihao/kaldi/egs/thchs30/thchs30-openslr`


然后修改/thchs30/s5/cmd.sh为本地跑：

```shell
export train_cms = run.pl
export decode_cmd = run.pl
export mkgraph_cmd = run.pl
export cuda_cmd = run.pl
​````

运行：

cd到s5目录下去跑

`sudo ./run.sh`

静候佳音

## 识别自己的wav

之后我们来到`tools/`下，去安装`./install_portaudio.sh`。

等安装完毕后我们到`src/`下，去 `make ext`去编译扩展程序。

## 找例子

万事具备，然后我们到egs下，打开voxforge文件夹，去把里面online_demo文件夹直接拷到thchs30下，之后我们在online_demo里建2个文件夹,一个为online-data，一个为work。

之后我们在online-data下建两个文件夹分别为audio和models。audio下放你的wav文件，models建一个名为tri1的文件夹，之后我们把`s5/exp/tri1`下的final.mdl和35.mdl(这是final.mdl的快捷方式)拷到`online-data/models/tri1`下。

然后把`s5/exp/tri/graph_word`里面的words.txt和HCLG.fst文件(如果你没有这两文件，说明你还没把模型训练好)拷到`online-data/models/tri1`下。

## 运行例子

我们在online_demo下`vi run.sh`。

之后我们把以下注释掉:

​```shell
if [ ! -s ${data_file}.tar.bz2 ]; then
    echo "Downloading test models and data ..."
    wget -T 10 -t 3 $data_url;
    if [ ! -s ${data_file}.tar.bz2 ]; then
        echo "Download of $data_file has failed!"
        exit 1
    fi
fi
```

然后在找到下面这句将其路径改成tri1:

```shell
ac_model_type=tri1
```

然后把下面的也改了注意看`online-wav-gmm-decode-faster`就行了：

```shell
online-wav-gmm-decode-faster --verbose=1 --rt-min=0.8 --rt-max=0.85\
--max-active=4000 --beam=12.0 --acoustic-scale=0.0769 \
scp:$decode_dir/input.scp $ac_model/final.mdl
```

## 运行

然后我们直接在online_demo下`./run.sh`

