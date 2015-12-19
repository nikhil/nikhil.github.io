---
layout: post
comments: true
title: Behavior Trees In Unity  
code:
- type: csharp 
  count: 0
- type: csharp
  count: 1
- type: csharp
  count: 2  

---
The projects were created by a team of 3 as part of a class project for Introduction to Computer Graphics at Rutgers University.


##Video 

<iframe width="480" height="270" src="https://www.youtube.com/embed/HhCYzRJoxO8" frameborder="0" allowfullscreen></iframe>


##Behavior Tree

You can play the Unity by clicking on the link 
<a href="/B2Game.html"> here.</a>

<a href="/B2Game.html"><img src="/images/Graphics/B2Start.png" alt="B2Start"/></a>

<i class="fa fa-gamepad"></i> **Controls** 
After the zombie introduction in the story, use the `Left Mouse Button` to click a location for the participant to move. You will need to run to the end of the map, into the military base by the barrel in order to win the game. If the zombie catches up to you the game is over.
{: .notice}



##Description

In this project, we used implemented a Behavior Tree using the KADAPT library in order to create our simulation. We also using the Final Inverse Kinematics library on Unity for creating animations.
The game starts out with the main character Daniel and his friends inside a small house. Daniel claps his hands and the door to the house opens.

<img src="/images/Graphics/B2Start.png" alt="B2Start"/>

Daniel and his friends walk out. His friends begin dancing, clapping, and cheering.

<img src="/images/Graphics/B2Dance.png" alt="B2Dance"/>

But then a Zombie appears and slams Daniel's friends with his super-powered baskballs.

<img src="/images/Graphics/B2Fear.png" alt="B2Fear"/>

Daniel cowers in fear and the user can now take control of him. Left click will allow the user to move Daniel to different locations and positions. To win, the user must control Daniel through the forest, the village, and into the entrance of a military fortress, which guarantees safety.
Here the user is making Daniel run through the forest and toward the village.

<img src="/images/Graphics/B2Run.png" alt="B2Run"/>

When the user successfully reaches the safety of the fortress, then the winning message is displayed.

<img src="/images/Graphics/B2UserWon.png" alt="B2UserWon"/>

But if the Zombie touches Daniel before that happens....

<img src="/images/Graphics/B2ZombieWon.png" alt="B2ZombieWon"/>

Daniel drops dead and the game ends. The basketball Zombie has successfully slammed Daniel into the ground.
Animations and Inverse Kinematics were used to create the actions of clapping, dancing, dying, picking up, and trembling in fear. A diagram of the behavior tree we implemented can be found below:

<img src="/images/Graphics/B2BehaviorTree.png" alt="B2BehaviorTree"/>

 The tree is consisted of multiple processes happening in parallel. There is a method which starts the static story where the user claps to open the door, the door opens and the user and friends go outside. The door has its own separate tree that is waiting for a clap to open. The friends have their own tree running in parallel with the others. The function of this tree is to make the friend scared when a zombie is nearby and to kill the friend when the zombie is close enough. The User has these functionalities in his tree as well. There is another tree which works on the user interface and monitors for clicks and then moves the player to that position. The zombie is controlled by the tree to attack the friends and target the player. The win condition is constantly monitored. Once the user reaches in range of a specific point at the end of the map the user wins.

More about the ADAPT/KADAPT libraries for Unity can be found <a href="http://www.cs.rutgers.edu/~mk1353/pdfs/2013-tvcg-adapt-preprint.pdf">here.</a>

##Implementation

Our project was implemented using KADAPT and Final IK. I will briefly describe how we implemented the affordance clapping:

{% highlight C# linenos %}

protected Node ParticipantOpenDoor()
	{
		return
			new Sequence(GoToPosition(participant, clapperPoint),
						 FacePosition(participant,door.transform),
						 participant.GetComponent<BehaviorMecanim>().ST_PlayHandGesture ("CLAP", 1000),
						 new LeafWait (1000),
						 GoToPosition(participant, partyObservationPoint),
						 FacePosition(participant, friends[dancerIndex].transform));
	}

protected Node AssertAndWaitForClap()
	{
		return new DecoratorLoop(
			new Sequence(
			new DecoratorInvert(
			new DecoratorLoop ((new DecoratorInvert (
			new Sequence(this.WaitForClap ()))))),
			new LeafWait(1000),OpenDoor()));
	}

protected Node WaitForClap()
	{
		return new LeafAssert (() => participant.GetComponent<Animator>().GetBool("H_Clap"));
	}

	protected Node OpenDoor()
	{
		Vector3 doorPosition = door.transform.position;
		doorPosition.y = doorPosition.y + 5;
		return new Sequence(new LeafInvoke (() => StartCoroutine(MoveObject(door.transform,
																			doorPosition,
																			4f))),
							new LeafWait(1000),
							HandleFriends());
	}
{% endhighlight %}


The ParticipantOpenDoor node tells Daniel to walk to a certain position (clapperPosition), faces a certain position, and performs the CLAP gesture. The AssertAndWaitForClap Node sees the clap complete, waits 1 second, and calls OpenDoor to open the door of the house.

Mouse controls were implemented by:


{% highlight C# linenos %}

	void PointOnMap()
	{
		Val<Ray> ray = Val.V (() => Camera.main.ScreenPointToRay (Input.mousePosition));
		RaycastHit hit;
		Physics.Raycast (ray.Value, out hit, 100);
		Val.V (() => Physics.Raycast (ray.Value, out hit, 100));
		positionToMove = (Val.V (() =>  hit.point)).Value;
		partyObservationPoint.transform.position  = (Val.V (() =>  positionToMove)).Value;
		UpdateParticipantPosition();
	}

	void UpdateParticipantPosition()
	{
		Val<Vector3> position = positionToMove;
		participant.GetComponent<SteeringController> ().Target = position.Value;
	}

	

	protected Node AssertClickInMap()
	{
		return
			new Sequence(new DecoratorInvert(new DecoratorLoop(new DecoratorInvert(new Sequence(this.CheckClickInMap())))),
						 new LeafInvoke(() => PointOnMap ())
						 //, new LeafInvoke(() => UpdateParticipantPosition ())
						 );
	}

{% endhighlight %}

##Custom Node

Our custom node is called MyNode, and it allows LeafInvoke to take in functions with a single parameter. This can work as a serial generator. For example:

{% highlight C# linenos %}

new MyNode((i) => GoToPosition(friends[i], x))
	
{% endhighlight %}

This works similarly to LeafInvoke but allows the user to send in an index to control the array that is a parameter for GoToPosition. This is useful when you want a list of objects to perform the same action, such as people going to a certain position.

<i class="fa fa-github-alt"></i> Github Link: <a href="https://github.com/CG-F15-9-Rutgers/UnityProjects/tree/master/BAssignments/B3"> Here </a>
