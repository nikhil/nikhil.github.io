---
layout: post
comments: true
title: BTK, A tool for reading C3d files
code:
- type: file
  file: C3d File
  count: 0

- type: packages
  count: 1

- type: bash
  count: 2

- type: bash
  count: 3

- type: python
  count: 4

- type: python
  count: 5

- type: python
  count: 6

- type: python
  count: 7

- type: python
  count: 8
 
---

I am currently doing research on the differences between Barefoot running and Shod running(running with shoes). I am working with software known as the `AnyBody Modeling System` using the `Gait lower Extremity model`. This allows me to input motion captured data, which is givin in the form of a `.c3d` file. However, I ran into a problem where I wanted to modify the .c3d file but I could not find anyt decent software to allow me to do this. I was able to find `btk` a `python` package that allows me to modify .c3d files.


##C3d files

###What are c3d files:

According to the c3d main page:
<blockquote>
The C3D format is a public domain, binary file format that has been used in Biomechanics, Animation and Gait Analysis laboratories to record synchronized 3D and analog data since the mid 1980's.  It is supported by all 3D major Motion Capture System manufacturers, as well as other companies in the Biomechanics, Motion Capture and Animation Industries.<a href="http://www.c3d.org/"><i class="fa fa-file"></i></a></blockquote>

###File Structure
The structure of the C3d file is as follows:

{% highlight text linenos%}

*.c3d/
	├── Header
	|    ├── File type
	|    ├── Data type
	|    ├── 3D Points
	|    ├── Analog Channels
	|    ├── First Frame
	|    ├── Last Frame
	|    ├── Start Record
	|    ├── AV ratio
	|    ├── Video rate
	|    └── Max interpolation Gap
	├── Events
	├── Groups
	|    |	├── Point
	|    |	├── ...
	|    |	├──Labels
	|    |	└── ...
	|    |
	|    ├── Analog
	|    ├── Seq
	|    ├── Manufacturer
	|    └── Force Platform
	├── Analog data
	└── Point data

{% endhighlight %}




##Instalation

## What is Btk?
According to the BTk site on google code:
<blockquote>
BTK is an open-source and cross-platform library for biomechanical analysis. BTK read and write acquisition files and can modify them. All these operations can be done by the use of the C++ API or by the wrappers included (Matlab, Octave, and Python).<a href="https://code.google.com/p/b-tk/"><i class="fa fa-cogs"></i></a>
</blockquote>



##Python packages needed



{% highlight python linenos %}
numpy
Matplotlib
BTK
{% endhighlight %}



To install python2 packages you need to run pip.
For example to install numpy, you need to run:


{% highlight python linenos %}
pip install numpy
{% endhighlight %}



<i class="fa fa-warning"></i> **Caution:** BTK is the exception here. I was not able to get btk working through pip. BTK also seems to be architectural dependent as it was not installing in my ARCH distribution. 
{: .notice}

I was able to get it working on Fedora 19 by installing the python wrapper package from <a href="https://code.google.com/p/b-tk/"> here. </a>


Then run:

{% highlight bash  linenos %}
yum localinstall packagename.rpm 
{% endhighlight %}



## Using b-tk to modify your file

You can import btk in python by specify your system path to the package or by
just using import depending on your installation.



{% highlight python  linenos %}
import sys
sys.path.append("path to your btk python package")
import btk

{% endhighlight %}





You then need to create a btk reader.



{% highlight python  linenos %}
reader = btk.btkAcquisitionFileReader()

{% endhighlight %}



Then you need to open the file and create a btk aquisition object.



{% highlight python  linenos %}
reader.SetFilename("sample.c3d") # open the c3d file
reader.Update()
acq = reader.GetOutput() # creates a btk aquisition object
 
{% endhighlight %}


You can then itterate through the Marker labels and Analog channels by doing:


{% highlight python  linenos %}

print('Marker labels:')
for i in range(0, acq.GetPoints().GetItemNumber()):
    print(acq.GetPoint(i).GetLabel(), end='  ')   
print('\n\nAnalog channels:')
for i in range(0, acq.GetAnalogs().GetItemNumber()):
    print(acq.GetAnalog(i).GetLabel(), end='  ') 
{% endhighlight %}



You can then set the value in a new c3d file clone or the original c3d file useing the
`SetValue` method.
Here is some code I did to cut off the force at some value.



{% highlight python  linenos %}
#modifies the values in the z axis 1st force plate

ana = clone.GetAnalog("Fz1")
valz1 = ana.GetValues()
scalez1 = ana.GetScale()
x = 0
y = float(sys.argv[2])
for forcez1 in np.nditer(valz1):
      if forcez1/scalez1 > y:
			ana.SetValue(x,y * scalez1)

      x = x + 1
{% endhighlight %}





