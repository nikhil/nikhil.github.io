---
layout: post
comments: true
title: Making My Gmail Script Execute Only  
code:
- type: bash
  count: 0

- type: bash
  count: 1

- type: file
  file: Shc files
  count: 2

- type: bash
  count: 3

- type: bash
  count: 4

- type: bash
  count: 5

- type: bash
  count: 6

---
Today I decided to `obfuscate` my `gmail script` which contains both my gmail
username and password. Initially, I had the files hidden as a dot file, however
I decided to use `shc` to convert my `.sh` file into a `binary` file. Then edit
the permissions to make it execute only.

<i class="fa fa-warning"></i> **Caution:** While this method prevents a casual observer from seeing the password in your file, it does not prevent someone who understands shc from reading your files. However, changing the script to a
hidden
execute only binary is significantly more protected than a hidden script file.
{: .notice}

## Installing and using shc

You can install shc by running:

{% highlight java linenos %}
pacman -S shc
{% endhighlight %}

Now run:

{% highlight java linenos %}
shc -f .mail.sh
{% endhighlight %}

This will give two files as the output:

{% highlight java linenos %}
.mail.sh.x.c
.mail.sh.x
{% endhighlight %}

Now test if your binary file works:

{% highlight java linenos %}
chmod +x .mail.sh.x
./.mail.sh.x
{% endhighlight %}

If you get `./mail.sh.x: Operation not permitted` , then you would need to
rerun shc. After searching online, I found that you can fix the probelm by
rerunning shc with additional `flags`:

{% highlight java linenos %}
shc -r -v -T -f .mail.sh
{% endhighlight %}

Now your binary script should work.

## Makeing the binary execute only

Now change to owner of your binary to root using `chown`:

{% highlight java linenos %}
 sudo chown root .mail.sh.x
{% endhighlight %}

Now use `chmod` to make the file execute only for those other than you:

{% highlight java linenos %}
sudo chmod 711 .mail.sh.x
{% endhighlight %}











