---
layout: post
comments: true
title: Computer Graphics Crowd Competition   
code:
- type: csharp 
  count: 0
- type: csharp
  count: 1
---

In this post we will discuss my team's submission to the Rutgers Computer Graphics class Crowd Competition using <a href="https://github.com/SteerSuite/SteerLite">SteerLite.</a>  

# Implementation

### Plane_Egress
Midterm points were hardcoded inside the plane. The agents follow the midterm points depending whether they are closer to the right or left entrance. This allows the agents to efficiently enter the plane and reach their goals with less bumping and interference.

<iframe width="480" height="270" src="https://www.youtube.com/embed/yTGPnK-ozzc" frameborder="0" allowfullscreen></iframe>

### Plane_Ingress
Similiar to Plane_Egress, midterm points were hardcoded outside the plane. The agents then follow the midterm points based on which exit (right or left) they are closest to, so they can reach the goal effectively and not get stuck in a wall.

<iframe width="480" height="270" src="https://www.youtube.com/embed/oPTCvhTBWiU" frameborder="0" allowfullscreen></iframe>

### Crowd_Crossing
Flags are set so that moving agents on the right reach and disappear before they cross the threshold. This allows the big agent to cross the threshold freely and reach its goal without any interruptions.

<iframe width="480" height="270" src="https://www.youtube.com/embed/0tLyZYlSwoo" frameborder="0" allowfullscreen></iframe>

### Office-Complex
A Star pathing was merged with social forces for this test case. This allows most of the agents to move and leave the office at the nearest exit effectively. The downside is that it takes long to calculate.

<iframe width="480" height="270" src="https://www.youtube.com/embed/7nVh2EWAz4Q" frameborder="0" allowfullscreen></iframe>

### Hallway-Four-Way-Rounded-Roundabout
We hardcoded extra goal points to ensure that every agent enters the circle to its right and goes around the circle counterclockwise (exactly like driving in a roundabout). This reduces the number of collisions between agents since their routes are organized.

<iframe width="480" height="270" src="https://www.youtube.com/embed/lC04v82CEEs" frameborder="0" allowfullscreen></iframe>

### Bottleneck-Squeeze
Half of the crowd uses regular social forces to enter the bottleneck. The other half goes through a row of hardcoded goal points in the middle of the bottleneck hallway. This allows the two crowds not to charge into one another.

<iframe width="480" height="270" src="https://www.youtube.com/embed/E1o2frA0Q48" frameborder="0" allowfullscreen></iframe>

### Doorway-Two-Way
Social forces and hardcoded goal points are used help the agents move farther away from one another as they are entering through the door. This helps them not to crash as much.

<iframe width="480" height="270" src="https://www.youtube.com/embed/rz4qlDQE1pA" frameborder="0" allowfullscreen></iframe>

### Double-Squeeze
We hardcoded goal points to force the agents to move farther away from each other and pass each other without crashing into one another.

<iframe width="480" height="270" src="https://www.youtube.com/embed/Nb2V56eOxvY" frameborder="0" allowfullscreen></iframe>

### Wall-Squeeze
We again hardcoded goal points to move the agents through the passageway safely without interfering with one another and causing collisions.

<iframe width="480" height="270" src="https://www.youtube.com/embed/tsJtswwkjAM" frameborder="0" allowfullscreen></iframe>

### Hallway-Two-Way
We used social forces with few hardcoded points in order to make the agents move effectively. 

<iframe width="480" height="270" src="https://www.youtube.com/embed/fgOxNxaZmwo" frameborder="0" allowfullscreen></iframe>

### Maze
We used the A Star planner that we merged with Social Forces (same as for office-complex) in order to control the agent. A Star gives the shortest path and there is only one agent, so computational time is reasonable. A Star is easily the best solution for getting a path through the maze.

<iframe width="480" height="270" src="https://www.youtube.com/embed/cbFFw44-wsI" frameborder="0" allowfullscreen></iframe>


### Other Implementation Details:

We used the reset function which is called once for each agent in order to decide the path that the agent will take for each test case. We used a global for the current test case name and use if statements in the reset function in order to decide which pathing to use. Most test cases have their own pathing function which is called in reset. This ensures that each test case has its own custom solution.

For example:

{% highlight C++ linenos %}

if (PathOfTestCase.find("maze") != std::string::npos || PathOfTestCase.find("office-complex") != std::string::npos) 
	{
		
		runAStarPlanning();
	}
	else if(PathOfTestCase.find("plane_egress") != std::string::npos)
        {
            hardCodePathEgress();   
        }
        else if(PathOfTestCase.find("plane_ingress") != std::string::npos)
        {
            hardCodePathIngress();   
        }

{% endhighlight %}

for choosing the pathing for each test case. An example of a hardcoded pathing function:

{% highlight C++ linenos %}

bool SocialForcesAgent::hardCodeDoorway()
{
 if(position().x == -10)
 {
    SteerLib::AgentGoalInfo goalPoint =_goalQueue.front();
    _goalQueue.pop();
    SteerLib::AgentGoalInfo newGoal1;
    SteerLib::AgentGoalInfo newGoal2;
    SteerLib::AgentGoalInfo newGoal3;
    newGoal1.targetLocation = Point(-10,0,2);
    newGoal2.targetLocation = Point(-2,0,2);
    newGoal3.targetLocation = Point(0,0,1);
    _goalQueue.push(newGoal1);
    _goalQueue.push(newGoal2);
    _goalQueue.push(newGoal3);
    _goalQueue.push(goalPoint);
    runLongTermPlanning();
    return true;
   
 }
}

{% endhighlight %}


# Creative Test Case

### Bus Stop Test Case

We created a realistic simulation of the Rutgers B bus stopping at the Busch Student Center during peak times. During peak times, many students are trying to push and shove their way between the B bus and the Busch Student Center. Students on the B bus will often crowd around the front doors and try to force their way out, while students trying to board the B bus will try to force their way through the side and back doors. Not all the students make it onto the B bus, but every student eventually makes it to the Busch Student Center (just as in real life).

For this simulation, we used our social forces planning with some hardcoded goal points to keep students in the right direction. The simulation features a lot of collisions and shoving, but this is realistic since our group has first hand experience with the madness that is boarding the B bus at peak hours. The steerbench score is 3872 due to collisions and energy spent. Students also act irrationally during this time which is why students would rather shove their way through the front door than try a side door which makes everything less efficient.

<iframe width="480" height="270" src="https://www.youtube.com/embed/cBhtjCloIk8" frameborder="0" allowfullscreen></iframe>


<i class="fa fa-github-alt"></i> Github Link: <a href="https://github.com/CG-F15-9-Rutgers/SteerLite"> Here </a>

