---
layout: post
comments: true
title: Batch Renaming with Bash 
code:
- type: bash
  count: 0

- type: bash
  count: 1

- type: bash
  count: 2

---

I recently had to rename 350+ files. So I decided it was time that I learned
how to rename files using bash scripting. There are multiple parts to this.
First you need to `create` and `itterate` through a list of files. Then you
need to send this into the `mv` command where you can rename the file using
special operations.

Here is an example of a script that removes all the `.tabular` extensions.

{% highlight bash  linenos%}

for f in *; 
do mv $f ${f%.tabular}; 
done 

{% endhighlight %}

Notice the `%` and the `f`. This is telling the script to remove the string
`.tabular` from the back f wich is your filename.
If you want something removed from the front of your file you should use `#`.
You may want to change the caseing of your file names. In that case, you can
just do:

{% highlight bash linenos%}

for f in *;
do mv $f `echo $f | tr '[:upper:]' '[:lower:]'`; 
done

{% endhighlight %}

Both of these scripts scans for all the files indicates by the `*`. If you
would like a specific file you can insert `*.txt` or `*.tabular`.

In addition, you can use the pipe command to get more complex. If you wanted to
remove the second occurrence of `_` from every file you can just pipe in `sed`
as so:

{% highlight bash linenos%}

for f in tss*;
do test ${f%.*} = $f && mv $f `echo $f | sed 's/_/./2'`; 
done

{% endhighlight %}



