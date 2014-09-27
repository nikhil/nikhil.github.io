---
layout: post
title: Adding Weather and Gmail info to my i3 Status 
comments: true
code:
- type: bash 
  count: 0

- type: file
  file: config
  count: 1

- type: file
  file: i3blocks.conf
  count: 2

- type: perl
  count: 3

- type: bash
  count: 4

- type: perl
  count: 5

- type: bash
  count: 6

---
Today I decided to use a program known as `i3blocks` to customize my `i3status`. It allows me to run outputs of scripts and colorize sections as well.

##Installing and configuring i3blocks

First you need to install i3blocks:

{% highlight java linenos %}

pacaur -S i3blocks-git

{% endhighlight %}

Then add the `i3blocks` script to your `i3` config file.

{% highlight bash linenos %}

bar 
	{
	status_command i3blocks -c ~/.i3/i3blocks.conf
	}

{% endhighlight %}

Now configure your i3blocks configuration file:

{% highlight java linenos %}

separator_block_width=14

[Weather]
command=~/.i3/IconicWeather.sh "08840"
interval=1800
color=#e97ac8


[mail]
command=echo -e " $(sh ~/GmailMonitor/mail.sh)"
interval=100
color=#7F00FF 

[disk-root]
label=:
command=/usr/libexec/i3blocks/disk /
interval=30
color=#1793D1


[disk-home]
label=:
command=/usr/libexec/i3blocks/disk $HOME
interval=30
color=#859900

[ssid]
label=
command=echo "$(iw dev wlo1 link | grep SSID | cut -d " " -f 2-)"
color=#d33682
interval=5

[wireless]
instance=wlo1
command=/usr/libexec/i3blocks/network
color=#00FF00
interval=10

[strength]
command=echo "$(iw dev wlo1 link | grep signal | cut -d " " -f 2-)"
interval=10
color=#cb4b16

[temp]
label=
command=echo "$(sensors coretemp-isa-0000 | awk '/Physical/ {print $4}')"
interval=10
color=#b58900


[battery]
command=~/.i3/battery BAT0

interval=30

[load]
label= 
command=/usr/libexec/i3blocks/load_average
interval=10
color=#6c71c4


[volume]
label=
command=~/.i3/volume-usb
interval=2
signal=10
color=#d70a53


[time]
label=
command=date '+%a %m-%d-%y %l:%M:%S %p'
interval=5
color=#50C878


{% endhighlight %}

If you want to remove the separator from a section just include the line: 
`separator=false` 


##Configuring the Scripts

I modified many of the scripts I found on the <a
href="https://github.com/vivien/i3blocks/wiki"> i3blocks wiki</a>

###The Weather Script

I wanted to display weather on my i3status but I wanted to include 
icons to indicate the type of weather at a location. So I piped the curl script
output in a `perl` script to print a `font-awsome` icon from a given weather
condition. 

{% highlight perl linenos %}
#!/bin/bash 

METRIC=0 #Should be 0 or 1; 0 for F, 1 for C
 
if [ -z $1 ]; then
echo
echo "USAGE: weather.sh <locationcode>"
echo
exit 0;
fi
 
curl -s http://rss.accuweather.com/rss/liveweather_rss.asp\?metric\=${METRIC}\&locCode\=$1 | perl -ne 'use utf8; if (/Currently/) {chomp;/\<title\>Currently: (.*)?\<\/title\>/; my @values=split(":",$1); if( $values[0] eq "Sunny" || $values[0] eq "Mostly Sunny" || $values[0] eq "Partly Sunny" || $values[0] eq "Intermittent Clouds" || $values[0] eq "Hazy Sunshine" || $values[0] eq "Hazy Sunshine" || $values[0] eq "Hot") 
{
my $sun = "";
binmode(STDOUT, ":utf8");
print "$sun";
}
if( $values[0] eq "Mostly Cloudy" || $values[0] eq "Cloudy" || $values[0] eq "Dreary (Overcast)" || $values[0] eq "Fog")
{
my $cloud = "";
binmode(STDOUT, ":utf8");
print "$cloud";
}
if( $values[0] eq "Showers" || $values[0] eq "Mostly Cloudy w/ Showers" || $values[0] eq "Partly Sunny w/ Showers" || $values[0] eq "T-Storms"|| $values[0] eq "Mostly Cloudy w/ T-Storms"|| $values[0] eq "Partly Sunny w/ T-Storms"|| $values[0] eq "Rain")
{
my $rain = "";
binmode(STDOUT, ":utf8");
print "$rain";
}
if( $values[0] eq "Windy")
{
my $wind = "";
binmode(STDOUT, ":utf8");
print "$wind";
} 
if($values[0] eq "Flurries" || $values[0] eq "Mostly Cloudy w/ Flurries" || $values[0] eq "Partly Sunny w/ Flurries"|| $values[0] eq "Snow"|| $values[0] eq "Mostly Cloudy w/ Snow"|| $values[0] eq "Ice"|| $values[0] eq "Sleet"|| $values[0] eq "Freezing Rain"|| $values[0] eq "Rain and Snow"|| $values[0] eq "Cold")
{
my $snow = "";
binmode(STDOUT, ":utf8");
print "$rain";
}
if($values[0] eq "Clear" || $values[0] eq "Mostly Clear" || $values[0] eq "Partly Cloudy"|| $values[0] eq "Intermittent Clouds"|| $values[0] eq "Hazy Moonlight"|| $values[0] eq "Mostly Cloudy"|| $values[0] eq "Partly Cloudy w/ Showers"|| $values[0] eq "Mostly Cloudy w/ Showers"|| $values[0] eq "Partly Cloudy w/ T-Storms"|| $values[0] eq "Mostly Cloudy w/ Flurries" || $values[0] eq "Mostly Cloudy w/ Snow")
{
my $night = "";
binmode(STDOUT, ":utf8");
print "$night";
}
print"$values[1]"; }'

{% endhighlight %}

### The Gmail Script

The gamil script in the wiki was not parsing the xml correctly. I fixed the
issue. The script shows the number of unread emails you have:

{% highlight bash linenos %}
USER=Your_Gmail_Username
PASS=Your_Password
 
COUNT=`curl -su $USER:$PASS https://mail.google.com/mail/feed/atom || echo "<fullcount>unknown number of</fullcount>"`
COUNT=`echo "$COUNT" | grep -oPm1 "(?<=<fullcount>)[^<]+" `
echo $COUNT
if [ "$COUNT" != "0" ]; then
    if [ "$COUNT" = "1" ];then
        WORD="mail";
    else
        WORD="mails";
    fi
fi
{% endhighlight %}

### The battery script

I made the batter script change icons when its `charging` vs `discharging`.

{% highlight perl linenos %}
...
if ($status eq 'Discharging') {
    
	$full_text .= " $percent% Bat";
} elsif ($status eq 'Charging') {
	$full_text .= "⚡ $percent% Chr";
}
... 

{% endhighlight %}

### The Volume Script

I had to modify the volume script, becuase I use an external sound card. This
means `amixer` will not be able to see a `master` device.

{% highlight perl linenos %}
...
amixer -c $CARD -M -D $MIXER get Headphone |  
... 
{% endhighlight %}

Here is the result of my configuration:<br>
You can view the full screen <a href="/images/i3status.png"> here. </a>

<img src="/images/i3status.png">









