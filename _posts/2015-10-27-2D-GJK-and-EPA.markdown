---
layout: post
comments: true
title: 2D Gilbert-Johnson-Keerthi and Expanding Polytope Algorithm  
code:
- type: bash 
  count: 0
- type: bash
  count: 1
- type: bash
  count: 2  
- type: cplusplus
  count: 3
- type: cplusplus
  count: 4
- type: cplusplus
  count: 5
- type: cplusplus
  count: 6
- type: cplusplus
  count: 7
- type: cplusplus
  count: 8
- type: cplusplus
  count: 9
- type: cplusplus
  count: 10

---

In this post we will discuss the implementation of GJK (Gilbert-Johnson-Keerthi) and EPA(Expanding Polytope Algorithm) in C++. The projects were created by a team of 3 as part of a class project for Introduction to Computer Graphics at Rutgers University.

##Demonstration

###Example 1

<img src="/images/Graphics/A2Example1.png" alt="A2Example1"/>

{% highlight bash linenos %}
$ ./steersim -testcase polygons1 -ai collisionAI

loaded module collisionAI
loaded module testCasePlayer
Initializing...
Preprocessing...
 NO collision detected between polygon No.0 and No.1
 NO collision detected between polygon No.0 and No.2
 NO collision detected between polygon No.0 and No.3
 Collision detected between polygon No.0 and No.4 with a penetration depth of 0.707107 and penetration vector of (0.707107,0,0.707107)
 Collision detected between polygon No.0 and No.5 with a penetration depth of 2 and penetration vector of (0,0,-1)
 NO collision detected between polygon No.1 and No.2
 NO collision detected between polygon No.1 and No.3
 NO collision detected between polygon No.1 and No.4
 NO collision detected between polygon No.1 and No.5
 NO collision detected between polygon No.2 and No.3
 NO collision detected between polygon No.2 and No.4
 NO collision detected between polygon No.2 and No.5
 Collision detected between polygon No.3 and No.4 with a penetration depth of 1 and penetration vector of (-1,0,0)
 NO collision detected between polygon No.3 and No.5
 Collision detected between polygon No.4 and No.5 with a penetration depth of 1.34164 and penetration vector of (-0.894427,0,-0.447214)
Simulation is running..

{% endhighlight %}

###Example 2

<img src="/images/Graphics/A2Example2.png" alt="A2Example2"/>


{% highlight bash linenos %}
$ ./steersim -testcase polygons2 -ai collisionAI

loaded module collisionAI
loaded module testCasePlayer
Initializing...
Preprocessing...
 Collision detected between polygon No.0 and No.1 with a penetration depth of 1 and penetration vector of (-0.8,0,0.6)
Simulation is running...

{% endhighlight %}

### Fixed test case of Example 2

<img src="/images/Graphics/A2Example3.png" alt="A2Example3"/>

{% highlight bash linenos %}
$ ./steersim -testcase polygons2-fixed -ai collisionAI

loaded module collisionAI
loaded module testCasePlayer
Initializing...
Preprocessing...
 NO collision detected between polygon No.0 and No.1
 NO collision detected between polygon No.0 and No.2
 NO collision detected between polygon No.1 and No.2
Simulation is running...

{% endhighlight %}

##Gilbert-Johnson-Keerthi Algorithm 

GJK is an algorithm which helps us decide if two shapes are colliding. You need
to first create a helper method which will calculate the point that will give
the maximum dot product in a particular direction.

{% highlight C++ linenos %}

int GetFarthestIndexInDirection(Util::Vector direction,const std::vector<Util::Vector>& ShapeA)
{
    double MaxDot = direction*ShapeA[0];
    int FarthestIndex = 0;
    
    
    for(unsigned int i = 1; i < ShapeA.size(); i++)
    {
        double CurrentDot = direction*ShapeA[i];
        
        if(CurrentDot>MaxDot)
        {
            MaxDot = CurrentDot;
            FarthestIndex = i;
        }
    }
    
    return FarthestIndex;
    
}

{% endhighlight %}

Next we need a support function which will get us our Minkowski Difference. The
Minkowski Difference is the subtraction of every point on the boundary of one
shape with every point on the boundary of another shape.

{% highlight C++ linenos %}

Util::Vector Support(const std::vector<Util::Vector>& ShapeA,const std::vector<Util::Vector>& ShapeB,Util::Vector direction)
{
    Util::Vector FirstPoint = ShapeA[GetFarthestIndexInDirection(direction,ShapeA)];
    Util::Vector newDirection = -1 * direction;
    Util::Vector SecondPoint = ShapeB[GetFarthestIndexInDirection(newDirection,ShapeB)];
    Util::Vector MinkowskiDifference = FirstPoint - SecondPoint;
    
    return MinkowskiDifference;
    
}

{% endhighlight %}

The GJK algorithm checks the Minkowski Difference. If these is an origin in the
simplex (a vector full of points), then there is a collision.

{% highlight C++ linenos %}

bool GJK(const std::vector<Util::Vector>& ShapeA,const std::vector<Util::Vector>& ShapeB, std::vector<Util::Vector>& simplex)
{
    Util::Vector DirectionVector(1,0,-1);
    simplex.push_back(Support(ShapeA, ShapeB, DirectionVector));
    Util::Vector newDirection = -1 * DirectionVector;
    
    while(true)
    {
        simplex.push_back(Support(ShapeA,ShapeB, newDirection));
        
        if(simplex.back() * newDirection <= 0)
        {
            
            return false;
        }
        else
        {
            if(CheckContainsOrigin(newDirection,simplex))
            {
                return true;
            }
        }
    }
}

{% endhighlight %}

The CheckContainsOrigin method checks if the origin is in a triangle formed by 3 points in the simplex The CheckContainsOrigin method is in our github linked on the bottom of this post.

Here is a nice Youtube visualization I found:

<iframe width="480" height="270" src="https://www.youtube.com/embed/kbwfplN1TiA" frameborder="0" allowfullscreen></iframe>

## Expanding Polytope Algorithm

The steps to compute EPA are as follows:

1. Get the nearest Edge to the origin

{% highlight C++ linenos %}

getNearestEdge(simplex, distance, normal, index);

{% endhighlight %}

2. Find a support point on that point in the direction of the edge's normal

{% highlight C++ linenos %}

Util::Vector sup = Support(shapeA, shapeB, normal);

{% endhighlight %}

3. Get the dot product of the support point and the edge's normal

{% highlight C++ linenos %}

float d = sup*normal;

{% endhighlight %}

4. If the difference between the distance to nearest edge and dot product is
   less than 0 then we have the penetration vector equal to the normal of the
   edge and the penetration depth equal to the distance.

{% highlight C++ linenos %}
 	
 if(d - distance <= 0)
    {
      penetration_vector = normal;
      penetration_depth = distance;
      return true;
      
    }

{% endhighlight %}

Helpful link for EPA: <a href="http://allenchou.net/2013/12/game-physics-contact-generation-epa/"> Here </a>


<i class="fa fa-github-alt"></i> Github Link: <a href="https://github.com/CG-F15-9-Rutgers/SteerLite/blob/master/steerlib/src/GJK_EPA.cpp"> Here </a>
