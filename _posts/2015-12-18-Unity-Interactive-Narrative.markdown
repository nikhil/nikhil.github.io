---
layout: post
comments: true
title: Unity Interactive Narrative
code:
- type: csharp
  count: 0
- type: csharp
  count: 1
- type: csharp
  count: 2  
- type: csharp
  count: 3
- type: csharp
  count: 4
- type: csharp
  count: 5

---

In this post we will discuss the implementation of an Interactive Narrative
Game in Unity. The projects were created by a team of 3 as part of a class project for Introduction to Computer Graphics at Rutgers University.
We used this Interactive Narrative to create a game in Unity called "Pacman: Come on and Slam Edition"

##Background Story

The story takes place in a post-apocalyptic world, ravaged by the Chaos Dunk which wiped out most life on Earth and turned some people into zombies. Daniel and his four friends, Michael, Charles, Jordan, and Barkley, are former basketball players hiding in a safe house in the countryside. Each day they take turns running into the surrounding hedges to scavenge for food for survival.


##Part 1

You can play the Unity Game by clicking on the link <a href="/B4Part1.html">here.</a>

<a href="/B4Part1.html"><img src="/images/Graphics/B4Start.png" alt="B4Start1"/></a>

<i class="fa fa-gamepad"></i> **Controls** 
This part is just a narration, there are no controls for this part.
{: .notice}

##Part 2

You can play the Unity Game by clicking on the link <a href="/B4Part2.html">here.</a>

<a href="/B4Part2.html"><img src="/images/Graphics/B4Start.png" alt="B4Start2"/></a>

<i class="fa fa-gamepad"></i> **Controls** 
You can use the `left mouse button` to control the player.
{: .notice}


##Part 3

You can play the Unity Game by clicking on the link <a href="/B4Part3.html">here.</a>

<a href="/B4Part3.html"><img src="/images/Graphics/B4Start.png" alt="B4Start3"/></a>

<i class="fa fa-gamepad"></i> **Controls** 
You can use the `left mouse button` to control the player. Use the `A` and `D`
keys to switch characters.
{: .notice}

##Vidoes

Here are the video recordings of each part. The audio has not been recorded.

Part 1

<iframe width="480" height="270" src="https://www.youtube.com/embed/MmHtww2Vz88" frameborder="0" allowfullscreen></iframe>

Part 2

<iframe width="480" height="270" src="https://www.youtube.com/embed/KROaXCStwE4" frameborder="0" allowfullscreen></iframe>

Part 3

<iframe width="480" height="270" src="https://www.youtube.com/embed/Iwxy634mc3c" frameborder="0" allowfullscreen></iframe>

##Narrative

To win the game, Daniel and his friends much collect all the food items in the arena before the zombies attack all of them. The game starts off with Daniel in control while his friends walk around randomly in the safe house (nonlinear and ambient characters). Daniel first walks up to the door and opens it by clapping. After exiting the safe house, Daniel walks around picking up food collectables as he goes (either autonomously or controlled by the user). The zombies in the surrounding area  each randomly spawn in one of three possible places (nonlinear complexity), to ensure a slightly different game experience each time. If Daniel picks up a basketball, a song remixed with the Space Jam theme song begins playing and Daniel becomes super-powered. He runs faster and can attack and kill zombies by running into them. The power only lasts for about a minute and then the game returns to normal. The zombies chase down the player. If a zombie gets too close to Daniel, he will die and control goes to a randomly selected friend. The user has five chances (five friends) to collect all of the food and win the game. If the user loses all five friends to the zombies, the player loses.

##Interaction

The user can control Daniel and his friends by left clicking with the mouse (if in part 2 or part 3). If in part 3, the user can switch between controlling a human and controlling a zombie by pressing 'a' or 'd' to cycle through the characters. This allows the user to play as zombies if they desire. The overhead shows how close each zombie or human is to one another in the hedge maze.
<br>
List of interactions and events:

<ol>
<li>Current human opens the door to the safe house by clapping</li>
<li>When a human gets close to a zombie, they perform a scared animation and move more slowly.</li>
<li>When close to a basketball, the human pick it up through an animation.</li>
<li>While holding a basketball, a Space Jam remix song plays in the background.</li>
<li>If a zombie reaches a human, the human falls into the ground, dead.</li>
<li>If a basketball holding human runs into a zombie, the zombie disappears and dies, respawning in its original location.</li>
<li>The humans currently inside the safe house wander around randomly.</li>
<li>When either all the food is collected or all the humans are dead, a message appears saying whether the humans or the zombies won.</li>
</ol>

<img src="/images/Graphics/B4BehaviorTree.png" alt="B4Behavior"/>

Above shows the entire interactive behavior tree for Part 3. For parts 1 and 2, the behavior tree is essentially the same, but part 1 does not have the nodes pertaining to user clicks and the nodes for switching characters, and part 2 does not have the nodes pertaining to user key presses to switch characters.

###Play Through 

<img src="/images/Graphics/B4StartGame.png" alt="B4StartGame"/>

The User starts the game as a player. The player can walk and pickup food items
(either by user clicks or autonomously).

<img src="/images/Graphics/B4ConfrontZombie.png" alt="B4ConfrontZombie"/>

When the player confronts the zombie, he will either become scared or be able
to attack the zombie if the powerup is in play. 

<img src="/images/Graphics/B4ZombieControl.png" alt="B4ZombieControl"/>

In part3, the zombie can be controlled by the user as well.

<img src="/images/Graphics/B4UserWon.png" alt="B4UserWon"/>

The user wins by collecting all the food items.

<img src="/images/Graphics/B4ZombieWon.png" alt="B4ZombieWon"/>

The zombie wins by defeating all the players.

###Brief Implementation Description

<blockquote>
Behavior Tree created with KADAPT: <a
href="https://github.com/mahyarkoy/KADAPT"> Link </a>
</blockquote>

The MainStory Story loop for the narrative.

{% highlight C# linenos %}

return new DecoratorLoop (
	new SequenceParallel (
	this.BuildMainTreeRoot (friends[0],0),
	this.BuildMainTreeRoot (friends[1],1),
	this.BuildMainTreeRoot (friends[2],2),
	this.BuildMainTreeRoot (friends[3],3),
	this.BuildMainTreeRoot (friends[4],4),
	this.AssertNextToPowerUp(friends[4],4),
	this.ZombieAssertAndGetHurt(zombie,1),
	this.ZombieAssertAndGetHurt(zombie1,2),
	this.AssertUserClickedA(),
	this.AssertUserClickedD(),
	this.BuildZombieTreeRoot(),
	this.BuildZombie1TreeRoot(),
	this.BuildDoorRoot(),
	this.UserControlsPlayer()
));

{% endhighlight %}

The spawn point of the zombie is randomized at the start of the game. This adds
nonlinearity to the game.

{% highlight C# linenos %}

spawnPos [0] = zombie1.transform.position;
spawnPos [1] = zombie1.transform.position + new Vector3 (40, 0, 0);
spawnPos [2] = zombie1.transform.position +  new Vector3 (70, 0, 0);
int spawnPoint = 0;
spawnPoint = Random.Range(0, 3);
Zombie1InitialPosition = new Vector3(spawnPos[spawnPoint].x,spawnPos[spawnPoint].y,spawnPos[spawnPoint].z);
print (spawnPoint + "\n");
zombie1.GetComponent<SteeringController> ().Warp (Zombie1InitialPosition);
spawnPos [0] = zombie.transform.position;
spawnPos [1] = zombie.transform.position + new Vector3 (60, 0, -180);
spawnPos [2] = zombie.transform.position +  new Vector3 (90, 0, 0);
spawnPoint = 0;
spawnPoint = Random.Range(0, 3);
ZombieInitialPosition = new Vector3(spawnPos[spawnPoint].x,spawnPos[spawnPoint].y,spawnPos[spawnPoint].z);
print (spawnPoint + "\n");
zombie.GetComponent<SteeringController> ().Warp (ZombieInitialPosition);

{% endhighlight %}

This loads the song when the `PlaySong` int is set to 1. This script is added
to an empty object in Unity with the component `AudioSource` having an
`AudioClip` link to your song file. 


{% highlight C# linenos %}

using UnityEngine;
using System.Collections;

[RequireComponent(typeof(AudioSource))]
public class Basketballsong : MonoBehaviour {

	public int PlaySong;
	private AudioSource audioS;
	private AudioClip Song;

	// Use this for initialization
	void Start () 
	{
		audioS = GetComponent<AudioSource>();
		Song = audioS.clip;
		PlaySong = 0;
	}
	
	// Update is called once per frame
	void Update ()
	{
	
		if (PlaySong == 1 && audioS.isPlaying != true)
		{
			audioS.PlayOneShot(Song, 0.7F);
		}
		if(PlaySong == 0 && audioS.isPlaying == true)
		{
			audioS.Stop();
		}
	}

}

{% endhighlight %}

<i class="fa fa-github-alt"></i> Github Link: <a href="https://github.com/CG-F15-9-Rutgers/UnityProjects/tree/master/BAssignments/B4"> Here </a>

###Credit

This game took a great deal of work to make, and we would like to give special thanks to the movie Space Jam (1996) and the independent video game, Barkley, Shut Up and Jam Gaiden for inspiring our concept, storyline, and music. 

We used these Space Jam mashups in our game:

Carry On Wayward Slam

<iframe width="480" height="270" src="https://www.youtube.com/embed/iBIf0OgZffU" frameborder="0" allowfullscreen></iframe>

U Can't Slam This

<iframe width="480" height="270" src="https://www.youtube.com/embed/E72cbmPDv9k" frameborder="0" allowfullscreen></iframe>

Ghostslammers

<iframe width="480" height="270" src="https://www.youtube.com/embed/awkUfIjl1R0" frameborder="0" allowfullscreen></iframe>

Can't Hold the Slam

<iframe width="480" height="270" src="https://www.youtube.com/embed/_CpWBXBAk1k" frameborder="0" allowfullscreen></iframe>





