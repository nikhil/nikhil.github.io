---
layout: post
comments: true
title: Unity Navigation and Animation 
code:
- type: csharp
  count: 0
- type: csharp
  count: 1
- type: csharp
  count: 2  
- type: csharp
  count: 3

---

In this post we will discuss the implementation of Navigation and Animation in
Unity. The projects were created by a team of 3 as part of a class project for Introduction to Computer Science at Rutgers University.

####Team Members:

* Nikhil Kumar
* Erin Corrado
* Daniel Bordak

##Navigation

You can play the Unity by clicking on the link 
<a href="/B1Part1Game.html"> here.</a>

<a href="/B1Part1Game.html"><img src="/images/Graphics/B1Start.png" alt="B1Start"/></a>

The Agents here are the purple capsules and the movable obstacles are the red
cylinders. 

<i class="fa fa-gamepad"></i> **Controls** 
Use the `Left Mouse Button` to click on any Agent or obstacle. They will turn
yellow when selected. Multiple agents can be selected. Once you select your
agent, use the `Right Mouse Button` to choose a target for the agents. Once you
select an obstacle, it can be moved using the `arrow keys`.
{: .notice}

Our first game focuses on using Unity's Navigation system and NavMesh to
create several objects. 

1. NavMesh Agents - objects which will navigate through the environment.
2. NavMeshObstacles - Movable objects which can block the Agents in the environment.
3. MeshLinks - Manual links between navigation meshes which allow agents to jump from
different heights.
4. Roaming Obstacles - Obstacles that automatically move through the
   environment.

###Play Through 

You can select an agent by clicking on the purple capsule using the Left Mouse
Button. 

<img src="/images/Graphics/B1Selected.png" alt="B1Selected"/>

After the agents are selected you can order them to navigate somewhere by
clicking anywhere on the map using the Right Mouse Button.

<img src="/images/Graphics/B1AgentsMoved.png" alt="B1AgentsMoved"/>

You can also move an obstacle by clicking on a red cylinder and using the arrow
keys. The cylinder in this picture is now blocking a doorway from the middle
room to the room on the left.

<img src="/images/Graphics/B1ObstacleMoved.png" alt="B1ObstacleMoved"/>

This obstacle forces the agents to take the long way when asked by the user to
enter the room.

<img src="/images/Graphics/B1LongWay.png" alt="B1Longway"/>

###Brief Implementation Description

<blockquote>
Unity Tutorial Used: <a
href="http://unity3d.com/learn/tutorials/topics/navigation"> Link </a>
</blockquote>

NavMeshAgent:

The  Unity object must have a navMeshAgent component. In `c#` we retrieved this component with:

{% highlight C# linenos %}
navMeshAgent = GetComponent<NavMeshAgent>();
{% endhighlight %}

We then set the agent to travel to the mouse click location using Raycast.

{% highlight C# linenos %}

RaycastHit hit;
...
if (Physics.Raycast (ray, out hit, 100)) {
	navMeshAgent.destination = hit.point;
	navMeshAgent.Resume ();
...
{% endhighlight %}

Camera Positon:

The Camera is set to always positioned at the middle of all the agents:

{% highlight C# linenos %}
...
Vector3 difference;
		sum = new Vector3 (0, 0, 0);
		for(int i=0; i<agents.Length; i++)
		{
			sum += agents[i].transform.position;
		}

		position = sum / (float)agents.Length;
...
{% endhighlight %}

The MeshLinks were created by using empty object to link two Navigation Meshes

##Animation

You can play the Unity by clicking on the link 
<a href="/B1Part2Game.html"> here.</a>

<a href="/B1Part2Game.html"><img src="/images/Graphics/B1WalkAnimation.png" alt="B1WalkAnimation"/></a>

<i class="fa fa-gamepad"></i> **Controls** 
Use the `arrow keys` to move the player around. The player will only walk.
Press `Left Shift` for the player to start running.  Use `space` to make the
player jump. 
{: .notice}

In Unity, Animation helps animate the character as it is in various states.
A transition occurs from one state to another when the parameter condition of a
transition is met. We made a simple character who can walk, run and jump.

###Play Through

Use the arrow keys to get the  character walking.

<img src="/images/Graphics/B1WalkAnimation.png" alt="B1WalkAnimation"/>

While the character is walking, hold down shift to get the character running. This is a picture of the character running.

<img src="/images/Graphics/B1RunAnimation.png" alt="B1RunAnimation"/>

You can press spacebar to make the character jump. This is a picture of the
character jumping.

<img src="/images/Graphics/B1JumpAnimation.png" alt="B1JumpAnimation"/>

###Brief Implementation Description

<blockquote>
Blend Trees 1-D Blending: <a
href="http://mecwarriors.com/2014/01/29/blend-trees-1-d-blending/"> Link </a>
</blockquote>



Triggers are parameters for animation control. For example, we used a trigger
to make our character jump.

{% highlight C# linenos %}
...

int jumpHash = Animator.StringToHash("Jump");

...
if(Input.GetKeyDown(KeyCode.Space))
		{
			animController.SetTrigger (jumpHash);
			...
		}

...
{% endhighlight %}




