---
layout: post
comments: true
title: A* PAthfinding   
code:
- type: cplusplus 
  count: 0
- type: cplusplus
  count: 1
- type: cplusplus
  count: 2  
- type: cplusplus
  count: 3
- type: cplusplus
  count: 4

---

In this post we will discuss the implementation of A* pathfinding  using <a href="https://github.com/SteerSuite/SteerLite">SteerLite.</a> The projects were created by a team of 3 as part of a class project for Introduction to Computer Graphics at Rutgers University. 

##Implementation

We implemented A* using two different heuristic methods: Euclidean distance and Manhattan distance. `Euclidean` distance is the straight line distance between any two points. `Manhattan` distance on the other hand is based only on horizontal and vertical paths on a grid.

###Manhattan distance

{% highlight C++ linenos %}

double AStarPlanner::Manhattan(Util::Point FirstPoint, Util::Point SecondPoint)
	{
		Util::Vector diff = FirstPoint - SecondPoint;
		return abs(diff.x) + abs(diff.y) + abs(diff.z);
	}
   
}

{% endhighlight %}

###Euclidean distance

{% highlight C++ linenos %}

double AStarPlanner::Heuristic(Util::Point a, Util::Point b)
	{
	...

	else {
	
	return (double)distanceBetween(a,b);
	
	}

	...

	}

{% endhighlight %}

###Example Videos

A* Manhattan First Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/tWRpKjDiLs8" frameborder="0" allowfullscreen></iframe>

A* Manhattan Second Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/bhKdOF7bb4c" frameborder="0" allowfullscreen></iframe>

A* Euclidiean First Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/yQ-P4AWsmOA" frameborder="0" allowfullscreen></iframe>

A* Euclidiean Second Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/UY-Y2VCb-Qc" frameborder="0" allowfullscreen></iframe>

##Handling Ties in F value

Ties in F values when searching for the lowest F value were handled by either
selecting the node with the higher or lower G value.

{% highlight C++ linenos %}

if(NodeMap.at(OpenSet[i]).f < lowestFScore)
{
  lowestFScore = NodeMap.at(OpenSet[i]).f;
  GScore = NodeMap.at(OpenSet[i]).g;
  lowestFIndex = i;
}
else if(NodeMap.at(OpenSet[i]).f == lowestFScore)
{
	 if(NextExpandingNodeTie == 1)
     {
       if(NodeMap.at(OpenSet[i]).f > GScore)
       {
         lowestFScore = NodeMap.at(OpenSet[i]).f;
         GScore = NodeMap.at(OpenSet[i]).g;
         lowestFIndex = i;
       }
    }
    else if(NextExpandingNodeTie == -1)
    {
      if(NodeMap.at(OpenSet[i]).f < GScore)
      {
        lowestFScore = NodeMap.at(OpenSet[i]).f;
        GScore = NodeMap.at(OpenSet[i]).g;
        lowestFIndex = i;
      }
						
						
    }
					
}
{% endhighlight %}

###Example Videos

A* Big G Favoring Manhattan First Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/LNvR3-OVpRQ" frameborder="0" allowfullscreen></iframe>

A* Big G Favoring Manhattan Second Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/zJ-KSq5hQ0I" frameborder="0" allowfullscreen></iframe>

A* Low G Favoring Manhattan First Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/s4QsdNZxnaw" frameborder="0" allowfullscreen></iframe>

A* Low G Favoring Manhattan Second Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/bxqTlOuNTg8" frameborder="0" allowfullscreen></iframe>

A* Big G Favoring Euclidiean First Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/3FKNFVCfArA" frameborder="0" allowfullscreen></iframe>

A* Big G Favoring Euclidiean Second Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/M8SD_iXbsFo" frameborder="0" allowfullscreen></iframe>

A* Low G Favoring Euclidiean First Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/Mdz0YVMbpsk" frameborder="0" allowfullscreen></iframe>

A* Low G Favoring Euclidiean Second Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/lFrIktZDeYU" frameborder="0" allowfullscreen></iframe>


Breaking the ties for smaller or bigger G values does not affect the length of the solution path at
all as would be expected. It does, however, change the number of nodes that are expanded. The
number of nodes that are expanded will be less than or equal to the number of nodes expanded in
the original A Star algorithm. The exact value depends on the structure of the environment, but
performance will never be worse than A Star.

##Increasing Diagonal Cost

The cost of taking Diagonal paths were increased.

{% highlight C++ linenos %}

TentativeScore = FromNode.g + distanceBetween(FromNode.point,CurrentPoint);
		
if(PART3)
{
   if(CurrentPoint.x != FromNode.point.x && CurrentPoint.z != FromNode.point.z) //if diagonal
   {
      TentativeScore = FromNode.g +  2*distanceBetween(FromNode.point,CurrentPoint);
   }
}

{% endhighlight %}

###Example Videos

A* Diagonally Weighted Manhattan First Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/gSFXmC-G3t4" frameborder="0" allowfullscreen></iframe>

A* Diagonally Weighted Manhattan Second Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/P9EWkLmsCfo" frameborder="0" allowfullscreen></iframe>

A* Diagonally Weighted Euclidiean First Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/HeyH7-ZmiiU" frameborder="0" allowfullscreen></iframe>

A* Diagonally Weighted Euclidiean Second Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/cUHBv5np4eY" frameborder="0" allowfullscreen></iframe>

Increasing the costs for diagonals worsens performance for Euclidean test cases. The agent is
less likely to take a diagonal step over a horizontal or vertical step, so the length of the solution
path larger than the one for A Star. It is usually faster for the agent to travel diagonally to reach
the goal, so performance is worsened. For Manhattan distance, the paths are unaffected. This is
expected because the agent will never travel diagonally when the heuristic is set to Manhattan
distance.

##Weighted Heuristic

Both the Manhattan and the Euclidean Heuristic were weighted with weights of 2,4,and 8.

{% highlight C++ linenos %}

double AStarPlanner::Heuristic(Util::Point a, Util::Point b)
{
	if(USE_MANHATTAN_DISTANCE) 
	{
     	return WEIGHT*Manhattan(a,b);
	} 
	else 
	{
		return WEIGHT*(double)distanceBetween(a,b);
	}
}

{% endhighlight %}



A* Heuristic of Weight 2 Manhattan First Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/D9VWXU2fTv4" frameborder="0" allowfullscreen></iframe>

A* Heuristic of Weight 2 Manhattan Second Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/OVZMKGCe6BY" frameborder="0" allowfullscreen></iframe>

A* Heuristic of Weight 4 Manhattan First Test Case 

<iframe width="480" height="270" src="https://www.youtube.com/embed/yuOAJ4GZQBE" frameborder="0" allowfullscreen></iframe>

A* Heuristic of Weight 4 Manhattan Second Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/1g3zVy1sEWY" frameborder="0" allowfullscreen></iframe>

A* Heuristic of Weight 8 Manhattan First Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/W3UpbDA_db8" frameborder="0" allowfullscreen></iframe>

A* Heuristic of Weight 8 Manhattan Second Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/FbGI7fJtFJw" frameborder="0" allowfullscreen></iframe>

A* Heuristic of Weight 2 Euclidean First Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/P3Q90NAbawo" frameborder="0" allowfullscreen></iframe>

A* Heuristic of Weight 2 Euclidean Second Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/pUlB2pDaKKo" frameborder="0" allowfullscreen></iframe>

A* Heuristic of Weight 4 Euclidean First Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/-A6jCe0NhCc" frameborder="0" allowfullscreen></iframe>

A* Heuristic of Weight 4 Euclidean Second Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/3hMQF6fqY1g" frameborder="0" allowfullscreen></iframe>

A* Heuristic of Weight 8 Euclidean First Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/e6akdEqhQdM" frameborder="0" allowfullscreen></iframe>

A* Heuristic of Weight 8 Euclidean Second Test Case

<iframe width="480" height="270" src="https://www.youtube.com/embed/IUD7BSNI-Vk" frameborder="0" allowfullscreen></iframe>

Using weighted A Star increases performance by significantly reducing the number of expanded
nodes. For example, in Search-2 Euclidean, adding a weight of 2 reduces the number of
expanded nodes from 2206 to 1026 while still finding the optimal path as in A Star. However,
while increasing the weights decreases the number of nodes you need to expand, it makes the
path found less likely to be the optimal one. Therefore, weighted A Star is not guaranteed to find
the optimal path. This can be seen in Search-2 Euclidean when the weight is 8. The optimal path
is 79. 799, but adding a weight of 8 finds a path of length 82.3848 although the number of
expanded nodes is greatly decreased.

<i class="fa fa-github-alt"></i> Github Link: <a href="https://github.com/CG-F15-9-Rutgers/SteerLite/blob/master/steerlib/src/AStarPlanner.cpp"> Here </a>




