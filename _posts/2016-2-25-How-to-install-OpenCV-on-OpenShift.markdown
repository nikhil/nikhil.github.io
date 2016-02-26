---
layout: post
comments: true
title: How to Install OpenCV on OpenShift   
code:
- type: bash 
  count: 0
- type: bash
  count: 1
- type: bash
  count: 2  
- type: file
  file: CMakeList.txt
  count: 3
- type: file
  file: CMakeList.txt 
  count: 4
- type: bash
  count: 5
- type: python
  count: 6
---

I wanted to deploy a python server on [OpenShift](https://www.openshift.com/) which would be able to use [OpenCV](http://opencv.org/) Image processing tools. I found a nice blog post [here](http://codingexodus.blogspot.com/2013/04/how-to-install-opencv-on-openshift.html), however I had multiple issues with it.

1. The link to the OpenCV source files was broken
2. It required a manual installation of python and cmake
3. Changed many important environment variables

I decided to make a more updated blog post on the installation.

## Installation

OpenShift now officially supports a python cartridge, so you do not need to
install it manually. This requires you to fix the installation parameters for
OpenCV (its okay I did that for you). Choose `Python 2.7` for your cartridge. Log in to your
application by using your apps ssh link. Now go into your temp directory.

{% highlight bash linenos=table %}

cd $OPENSHIFT_TMP_DIR

{% endhighlight %}

Now download and extract OpenCV

{% highlight javascript linenos=table %}

wget http://pkgs.fedoraproject.org/repo/pkgs/opencv/OpenCV-2.4.3.tar.bz2/c0a5af4ff9d0d540684c0bf00ef35dbe/OpenCV-2.4.3.tar.bz2
tar xvf OpenCV-2.4.3.tar.bz2

{% endhighlight %}

<i class="fa fa-warning"></i>  **Notice:** This is OpenCV version `2.4.3`. I only tested it on this version. Other versions might not work.
{: .notice}

Now we need to remove some files to prevent a `Disk quota exceeded` error from
occurring.

{% highlight javascript linenos=table %}

rm OpenCV-2.4.3.tar.bz2
cd OpenCV-2.4.3/
rm -r 3rdparty/
rm -r android
rm -r doc
rm -r data
rm -r ios
rm -r samples
rm -r apps
rm README
{% endhighlight %}

Now we have to edit `CMakeList.txt` so we do not get an error from the files we
just removed. We also need to make sure certain tests are not performed. Run
the command `vim CMakeList.txt`. You can use `:set number` to show the line
numbers in vim. Now turn off regression and performance tests on lines 155-156.
It should look like this:

{% highlight bash linenos=table %}

OCV_OPTION(BUILD_PERF_TESTS         "Build performance tests"                     OFF  IF (NOT IOS) )
OCV_OPTION(BUILD_TESTS              "Build accuracy & regression tests"           OFF  IF (NOT IOS) )

{% endhighlight %}

To ensure the program does not try to access the folders we just deleted comment out lines 448-456. It should look like this:

{% highlight bash linenos=table %}

# Generate targets for documentation
#add_subdirectory(doc)

# various data that is used by cv libraries and/or demo applications.
#add_subdirectory(data)
 
# extra applications
#add_subdirectory(apps)

{% endhighlight %}

Now we need to fix the installation parameters for
OpenCV so it installs on the python already installed by OpenShift (its okay I did that for you). Make sure you care in the `OpenCV` folder.

{% highlight javascript linenos=table %}
mkdir release
cd release

cmake ../OpenCV-2.4.3 -D BUILD_NEW_PYTHON_SUPPORT=ON -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=$OPENSHIFT_DATA_DIR -D PYTHON_LIBRARY=$LD_LIBRARY_PATH/libpython2.7.so -D CMAKE_INCLUDE_PATH=$OPENSHIFT_HOMEDIR/python/virtenv/include/python2.7 -D PYTHON_INCLUDE_DIR=$OPENSHIFT_HOMEDIR/python/virtenv/include/python2.7 -D PYTHON_PACKAGES_PATH=$OPENSHIFT_HOMEDIR/python/virtenv/lib/python2.7/site-packages -D PYTHON_EXECUTABLE=$OPENSHIFT_HOMEDIR/python/virtenv/bin/python -D WITH_OPENEXR=OFF -D BUILD_DOCS=OFF -DBUILD_SHARED_LIBS=ON ..

make
make install
{% endhighlight %}

## Testing

You should get no error messages when you type `import cv2`

{% highlight bash linenos=table %}
Python 2.7.8 (default, May 19 2015, 02:50:14) 
[GCC 4.4.7 20120313 (Red Hat 4.4.7-4)] on linux2
Type "help", "copyright", "credits" or "license" for more information.
>>> import cv2
>>> print cv2.__version__
2.4.3
{% endhighlight %}





