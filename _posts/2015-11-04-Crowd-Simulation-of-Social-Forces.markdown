---
layout: post
comments: true
title: Crowd Simulation of Social Forces  
code:
- type: bash 
  count: 0
- type: bash
  count: 1
- type: bash
  count: 2  
- type: bash 
  count: 3
- type: cplusplus
  count: 4
- type: cplusplus
  count: 5
- type: cplusplus
  count: 6
- type: cplusplus
  count: 7

---

<link rel="stylesheet" href="../css/katex.min.css"> 
<script src="../javascript/katex.min.js"></script>
<script src="../javascript/auto-render.min.js"></script>

In this post we will discuss the implementation of Social Forces in crowd
simulations using <a href="https://github.com/SteerSuite/SteerLite">SteerLite.</a> The projects were created by a team of 3 as part of a class project for Introduction to Computer Graphics at Rutgers University. 


##Demonstration

###Bottle Neck

<iframe width="480" height="270" src="https://www.youtube.com/embed/CQgaVsLDAIU" frameborder="0" allowfullscreen></iframe>


<i class="fa fa-info-circle"></i> **Info**
In this simulation a crowd of agents needs to pass through a narrow opening of
the room. The situation leads to many agents shoving each other to leave the
room.
{: .notice}

Bench Scores:


{% highlight bash linenos %}
$ ./steerbench Bottle_Neck.rec -detail -technique composite02

             total number of agents: 200
avg. number of collisions per agent: 18.25
    average time spent by one agent: 99.929
  average energy spent by one agent: 1306.86
 sum of instantaneous accelerations: 3230.26
(alpha, beta, gamma, delta) weights: (50,1,1,1)
                       weighted sum: 50*18.25 + 1*99.929 + 1*1306.86 + 1*3230.26 = 5549.54
                        final score: 5549.54

{% endhighlight %}



###One Way Hallway

<iframe width="480" height="270" src="https://www.youtube.com/embed/RuCOo32EpaQ" frameborder="0" allowfullscreen></iframe>

<i class="fa fa-info-circle"></i> **Info**
In this simulation a crowd of agents needs to pass through a hallway. The
agents are all traveling one direction. 
room.
{: .notice}

Bench Scores:

{% highlight bash linenos %}
$ ./steerbench One_Way.rec -detail -technique composite02

             total number of agents: 200
avg. number of collisions per agent: 0
    average time spent by one agent: 84.3483
  average energy spent by one agent: 1502.96
 sum of instantaneous accelerations: 12.0091
(alpha, beta, gamma, delta) weights: (50,1,1,1)
                       weighted sum: 50*0 + 1*84.3483 + 1*1502.96 + 1*12.0091 = 1599.32
                        final score: 1599.32
{% endhighlight %}


###Two Way Hallway

<iframe width="480" height="270" src="https://www.youtube.com/embed/U7gWIndARuI" frameborder="0" allowfullscreen></iframe>

<i class="fa fa-info-circle"></i> **Info**
In this simulation a crowd of agents needs to pass through a hallway. The
agents are traveling in both directions. 
room.
{: .notice}

Bench Scores:

{% highlight bash linenos %}
$ ./steerbench Two_Way.rec -detail -technique composite02

             total number of agents: 200
avg. number of collisions per agent: 0
    average time spent by one agent: 70.4994
  average energy spent by one agent: 1220.02
 sum of instantaneous accelerations: 220.666
(alpha, beta, gamma, delta) weights: (50,1,1,1)
                       weighted sum: 50*0 + 1*70.4994 + 1*1220.02 + 1*220.666 = 1511.18
                        final score: 1511.18

{% endhighlight %}

###Four Way Hallway

<iframe width="480" height="270" src="https://www.youtube.com/embed/1NwwyWxPtmk" frameborder="0" allowfullscreen></iframe>

<i class="fa fa-info-circle"></i> **Info**
In this simulation a crowd of agents need to pass through a four way
intersection.
{: .notice}

Bench Scores:

{% highlight bash linenos %}
$ ./steerbench Four_Way.rec -detail -technique composite02

             total number of agents: 400
avg. number of collisions per agent: 0.015
    average time spent by one agent: 76.1899
  average energy spent by one agent: 1291.71
 sum of instantaneous accelerations: 414.429
(alpha, beta, gamma, delta) weights: (50,1,1,1)
                       weighted sum: 50*0.015 + 1*76.1899 + 1*1291.71 + 1*414.429 = 1783.08
                        final score: 1783.08
{% endhighlight %}

#Implementation

####Sum of Forces:

$[ m_i\frac{dv_i}{dt} = F_{goal} + F_{agents} + F_{walls} ]$

This is the sum of all the forces that make up the Social Force.

{% highlight c++ linenos %}

Util::Vector acceleration = (prefForce + repulsionForce + proximityForce) / AGENT_MASS;

{% endhighlight %}



####Goal Directed Force:

$[ F_{goal} = m_i\frac{v_i^0(t)e^0_i(t) - v_i(t)}{\tau_i} ]$

This Force changes the speed and direction according to the direction of the
goal. 

{% highlight c++ linenos %}

Vector SocialForcesAgent::calcGoalForce(Vector _goalDirection, float _dt)
{
  Util::Vector goalForce = (((_goalDirection * PREFERED_SPEED) - velocity()) / _dt)/5;
  return goalForce;
    
}
{% endhighlight %}

####Agent Collision Avoidance Force:

$[ F_{agents} = \sum_{j \ne i }F_{ij} ]$

$[ F_{ij} = (A_ie^{\frac{r_{ij}-d_{ij}}{B_i}} + kg(r_{ij} - d_{ij}))n_{ij} + kg(r_{ij}-d_{ij})\Delta v^t_{ji}t_{ij} ]$

$( F_{ij} )$ is the sum of forces of agent j on agent i \\
R is the Radii \\
d is the distance between centers of mass \\
A and B are constants

{% highlight c++ linenos %}

Util::Vector SocialForcesAgent::calcAgentRepulsionForce(float dt)
{
...
for(std::set<SteerLib::SpatialDatabaseItemPtr>::iterator neighbor = _neighbors.begin(); neighbor != _neighbors.end(); neighbor++)
{
    if( (*neighbor)->isAgent() )
      tempAgent = dynamic_cast<SteerLib::AgentInterface *> (*neighbor);
    
    else
      continue;
    
    if( (id() != tempAgent->id() ) && (tempAgent->computePenetration(this->position(),this->radius()) > 0.000001))
    {
     agent_repulsion_force = agent_repulsion_force + ( tempAgent->computePenetration(this->position(),this->radius()) * _SocialForcesParams.sf_agent_body_force * dt) * normalize(position() - tempAgent->position());  
     
    }
     
}
...
}

{% endhighlight %}


####Wall Collision Avoidance Force:

$[ F_{walls} = \sum_{j \ne i}F_{iW} ]$

$[ F_{iW} = (A_ie^{\frac{r_{i}-d_{iW}}{B_i}} + kg(r_{i} - d_{iW}))n_{iW} - kg(r_{i}-d_{iW})(v_i * t_{iW})t_{iW}]$

$( F_{iW} )$ is the sum of forces of wall W on agent i  \\
R is the Radii  \\
d is the distance between centers of mass \\
A and B are constants

{% highlight c++ linenos %}

Util::Vector SocialForcesAgent::calcWallRepulsionForce(float dt)
{
...

for (std::set<SteerLib::SpatialDatabaseItemPtr>::iterator neighbor = _neighbors.begin(); neighbor!=_neighbors.end(); neighbor++)
{
	 if(!(*neighbor)->isAgent())
	   tmp_ob = dynamic_cast<SteerLib::ObstacleInterface *>(*neighbor);
	 
	 else
	   continue;
	 
	 if(tmp_ob->computePenetration(this->position(), this->radius()) > 0.000001)
	 {
	  Util::Vector wallNormal = calcWallNormal(tmp_ob);
	  std::pair<Util::Point,Util::Point> line = calcWallPointsFromNormal(tmp_ob, wallNormal);
	  std::pair<float, Util::Point> min_stuff = minimum_distance(line.first, line.second, this->position());
	  wall_repulsion_force = wall_repulsion_force +  wallNormal * (min_stuff.first + this->radius()) * _SocialForcesParams.sf_body_force*dt;

	 }
	  
}

...
}

{% endhighlight %}

<i class="fa fa-github-alt"></i> Github Link: <a href="https://github.com/CG-F15-9-Rutgers/SteerLite/blob/master/socialForcesAI/src/SocialForcesAgent.cpp"> Here </a>

<script>

document.addEventListener("DOMContentLoaded", function(event) { 

	renderMathInElement(
          document.body,
          {
              delimiters: [
                  {left: "$[", right: "]$", display: true},
                  {left: "$(", right: ")$", display: false},
             ]
          }
      );
});

</script>


