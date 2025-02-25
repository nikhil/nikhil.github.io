---
layout: page
permalink: /projects/
title: Projects
tagline: 
tags: [projects]
modified: 9-19-2014
comments: true
image:
  feature: rabbit.jpg
  credit: A Rabbit from my backyard.
programs:
- name: FreeBird
  url: http://freebird.kumarcode.com/ 
  description: Helps allow Doctors easily monitor and visualize the mental health of patients, dietary, and sleeping pattern. Correlates nutrition and sleep with the changes in mood.
  images: 
  - image: FreeBirdResult.png 
  location: Rutgers Hackathon Spring 2015
  tags:
  - name: Python
  - name: Twitter
  - name: MongoDB
  - name: Openshift
  - name: USDA
  - name: Mood
  - name: Health
  prize: Won the Twitter award
- name: PostalPortraits
  url: https://github.com/Erin-Corrado/PostalPortraits
  description: Analyzes the content of a conversation thread on a users email and then transfers those changes through image modifications using the Pillow image manipulation tool.
  images: 
  - image: huskybefore.jpg
  - image: huskyafter.jpg
  location: Rutgers Hackathon Spring 2014
  tags:
  - name: Python
  - name: Context.io
  - name: Pillow
  - name: Email
  prize: Won the Context.io award
- name: SwitchBeat
  url: http://kumarcode.com/switchbeat.html
  description: Dynamically Mashups any two songs
  images: 
  - image: SwitchBeat1.png
  - image: SwitchBeat.png
  location: Independent
  tags:
  - name: Javascript
  - name: Mashups
  - name: EchoNest
- name: MyoMelodies
  url: https://github.com/Erin-Corrado/MyoMelodies
  description: Lua script that allows you to control Foobar2000 from the Myo.
  images: 
  - image: foobar.png
  - image: MYO-armband.jpg
  location: Penn Apps Fall 2014
  tags:
  - name: Lua
  - name: Myo
  - name: Foobar2000
- name: MathArt.js
  url: https://github.com/nikhil/MathArt.js
  description: Creates random Math art using javascript. Used in my website. It randomizes the amplitude, frequency, and phase shift numbers.
  images: 
  - image: MathArt2.gif
  location: Independent
  tags:
  - name: Javascrpt
  - name: Math
  - name: Trig
  - name: canvas
- name: Zork
  url: http://zork.kumarcode.com
  description: Using php to host Zork on heroku
  images: 
  - image: zork.png 
  location: Independent
  tags:
  - name: Php
  - name: Bash
  - name: Heroku
  - name: Text Adventure
- name: Slam
  url: https://github.com/nikhil/Slam
  description: Streaming Live Awesome Music. Python script for streaming music from iheartradio.
  images: 
  - image: iheart.png
  location: Independent
  tags:
  - name: Python
  - name: Music
  - name: Streaming
  - name: iHeartRadio
- name: c3dfileeditor
  url: https://github.com/nikhil/c3dfileeditor
  description: A a script that reads, plots data as well as modifies c3d files using the btk library. Used with The Anybody Modeling System 
  images: 
  - image: anybody.jpg
  location: Research
  tags:
  - name: Python
  - name: Gait Analysis
  - name: Anybody Modeling System
  - name: Btk
- name: galaxytocummerbund 
  url: https://github.com/nikhil/galaxytocummerbund
  description: A bash script that batch renames all the output files downloaded from the Galaxy server so that it can be used to run CummeRbund.
  images: 
  - image: galaxy.png
  - image: rna.jpg
  location: Research
  tags:
  - name: Bash 
  - name: Renaming
  - name: Rna Seq
  - name: CummeRbund
  - name: Galaxy
- name: GeoKeywords
  url: https://github.com/ashleyweaver/hackNY2013
  description: Uses NY Times API to get cool stories based on where you are.
  images: 
  - image: Nytimes.png
  location: Hack Ny Fall 2013
  tags:
  - name: Python
  - name: Nytimes
  - name: location
  - name: Wordle
- name: Resume 
  url: https://github.com/nikhil/Resume
  description: My personal resume in Latex 
  images: 
  - image: latex.png
  location: Independent
  tags:
  - name: LaTeX
  - name: Resumes
- name: HubbleConstantMonitor 
  url: https://github.com/nikhil/HubbleConstantMonitor.js
  description: Monitors the Hubble Constant on wikipedia 
  images: 
  - image: hubble.jpg
  location: Independent
  tags:
  - name: javascript
  - name: Hubble Constant
- name: IconicWeather 
  url: https://github.com/nikhil/IconicWeather
  description: A script used with i3blocks that monitors weather from Accuweather and displays the weather on your bar. 
  images: 
  - image: aw-logo.png
  - image: i3bar.png
  location: Independent
  tags:
  - name: Perl
  - name: Weather
  - name: i3blocks
  - name: Font Awsome
- name: GmailMonitor 
  url: https://github.com/nikhil/GmailMonitor
  description: Monitors new emails from Gmail 
  images: 
  - image: Gmail_logo.png
  - image: i3bar.png
  location: Independent
  tags:
  - name: Shell
  - name: i3blocks
  - name: Gmail

---

{% for project in page.programs%}

<div class="projectBox">
        
		<h1><a class="projectTitle" href="{{project.url}}">{{project.name}}</a></h1>

		{% for picture in project.images %}

                <img class="projectImage" src="{{ site.url }}/images/projects/{{picture.image}}" width="594" height="334" >
            
        {% endfor %}
        <span class="projectDescrip"> {{project.description}}</span>
		<p class ="projectTags"><i class="fa fa-tags fa-lg fa-fw"></i>&nbsp;&nbsp;&nbsp;{% for tag in  project.tags%}<code>{{tag.name}}</code>{% endfor %}<br>
		<em>
		{% if project.location %}
		<span class="projectLocation"><i class="fa fa-map-marker fa-lg fa-fw"></i>&nbsp;&nbsp;&nbsp;{{project.location}}</span>
		{% endif %}<br>	
        {% if project.prize %}
          <span class="projectDescrip"><i class="fa fa-trophy fa-lg fa-fw"></i>&nbsp;&nbsp;&nbsp;{{project.prize}}</span>
        {% endif %}
		</em>
		</p>
		
    </div>


<br>
{% endfor %}




