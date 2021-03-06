---
layout: post
title: Install MXNet on CentOS 7
tags: [mxnet, centOS]
published: true
---

MXNet is a lightweight, portable, and flexible deep learning framework with support to many programming languages such as C++, python, scala, julia, javascript, and R. 

Since MXNet [installation guide](http://mxnet.readthedocs.io/en/latest/how_to/build.html) does not provide a guideline to install MXNet on CentOS 7, I write an installation guideline for CentOS 7. Here are the steps to build MXNet on CentOS 7:


#### 1. Install dependencies
{% highlight shell %}
yum groupinstall "Development Tools" 
yum install atlas-devel opencv-devel
{% endhighlight %}

Atlas is installed in `/usr/lib64/atlas`.

#### 2. Clone MXNet project
{% highlight shell %}
git clone --recursive https://github.com/dmlc/mxnet
{% endhighlight %}

  
#### 3. Modify MSHADOW_LDFLAGS in `mshadow/make/mshadow.mk` (line 67) from `-lcblas` to `-L/usr/lib64/atlas -lsatlas`
{% highlight shell %}
...
else ifeq ($(USE_BLAS), atlas)
        MSHADOW_LDFLAGS += -L/usr/lib64/atlas -lsatlas
...        
{% endhighlight %}

  
#### 4. Make
{% highlight shell %}
make -j$(nproc)
{% endhighlight %}

  
#### 5. MXNet Python package installation
Follow official MXNet [Python package installation](http://mxnet.readthedocs.io/en/latest/how_to/build.html#python-package-installation) guide
