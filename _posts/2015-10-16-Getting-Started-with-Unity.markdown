---
layout: post
comments: true
title: Getting Started with Unity 
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

---

In this post I will discuss the implementation of my first Unity project. The projects were created as a class project for Introduction to Computer Graphics at Rutgers University.

###Roll A Ball

You can play the Unity game by clicking on the link 
<a href="/UnityProjectBuilds/B0Build/index.html"> here.</a>

<a href="/UnityProjectBuilds/B0Build/index.html"><img src="/images/Graphics/B0Start.png" alt="B0Start"/></a>

<i class="fa fa-gamepad"></i> **Controls** 
The key controls for the first player are: `left` arrow key to go left, `right` arrow key to
go right, `forward` arrow key to go forward, `back` arrow key to go back, and `right control` to jump. The
controls for the second player are: `a` to go left, `w` to go forward, `d` to go right, `s` to go back, and `tab` to
jump.
{: .notice}

##Video 

<iframe width="480" height="270" src="https://www.youtube.com/embed/C4racQM0hG8" frameborder="0" allowfullscreen></iframe>



###Description

This game is a two player game. Two players must compete to get the most points by picking up
the yellow rotating cubes. The players are two rolling balls. The red ball is the first player, while the blue
ball is the second player. The arena has a main platform and 4 raised side platforms. The raised side
platforms are connected to the main platforms with 4 ramps. The platform was designed to look as if it
was floating in space. Although the platform has high walls, the program is designed to handle when a player falls off
the platform. When a player falls off the platform, the other player automatically wins and the game is
over. Each cube is worth 5 points, a crash on the wall will make you lose two points. When two players
collide, the player with the lowest altitude will lose 3 points. If the players have the same altitude then
no player will lose points.

Some of the features in this game:

1. The Game is two player, and can be played using keyboard keys on one keyboard. I felt using the
keyboard will be the best option, since a keyboard/mouse pair or a keyboard/joystick pair will
be unfair to the players since the controls are fairly different.
2. There is a TimeStatus update which lets you know when there is a minute left, 30 seconds left
and 10 seconds left.
3. There is a player status update for each player whenever they pick up a cube, collide into a wall,
win a collision with another player, loose a collision with another player, and tie a collision with
another player.
4. The Game is hosted online and playable with Javascript and Html5 

###Playthrough

<img src="/images/Graphics/B0Camera.png" alt="B0Start"/>

The camera repositions itself and its focus based on the distance between the
two players.

<img src="/images/Graphics/B0PlayerFell.png" alt="B0PlayerFell"/>

If a player falls, that player looses and the other one wins.

<img src="/images/Graphics/B0CollisionTie.png" alt="B0CollisionTie"/>

If two players crash into each other with equal height, the collision is a tie
and no points are lost.

<img src="/images/Graphics/B0WinCollision.png" alt="B0WinCollision"/>

A player can jump onto another player and win the collision the other player
to loose points.



###Brief Implementation Description

<blockquote>
Unity Tutorial Used: <a href="https://unity3d.com/learn/tutorials/projects/roll-ball-tutorial"> Link </a>
</blockquote>

The  Unity game was written in `c#`. Here I describe how I implemented some key
features.

Picking up an object:

{% highlight C# linenos %}
...
void OnTriggerEnter(Collider other) 
{
 if (GameOver == true)
 {
    return;
 }
 if (other.gameObject.CompareTag ("Pick Up"))
 {
    other.gameObject.SetActive (false);
    score = score + 5;
    SetCountText();
    StartCoroutine(DisplayQuickMessage(PlayerStatus,"Player 1 picks a cube +5",0.3f));
 }
}
...
{% endhighlight %}

Rotating Objects:

{% highlight C# linenos %}
...
void Update ()
{
	transform.Rotate (new Vector3 (15, 30, 45) * Time.deltaTime);
}
...
{% endhighlight %}


Keeping Score:

This script controls the points for player1. The implementation for player 2 is
similar.   

{% highlight C# linenos %}
...
void OnCollisionEnter(Collision CollObject)
{
 if (GameOver == true)
 {
     return;
 }
 if (CollObject.gameObject.tag == "Ground") 
 {
    isJumping = false;

 }
 else if (CollObject.gameObject.tag == "Wall") 
 {
    score = score - 2;
    SetCountText();
    StartCoroutine(DisplayQuickMessage(PlayerStatus,"Player 1 hits a wall -2",0.3f));
 }
 else if (CollObject.gameObject.tag == "Player1") 
 {
    if (CollObject.gameObject.transform.position.y == objectRb.transform.position.y || System.Math.Abs(CollObject.gameObject.transform.position.y - objectRb.transform.position.y) <.03f)
    {
        StartCoroutine(DisplayQuickMessage(PlayerStatus,"Player 1 ties Collision",0.3f));
    }
    else if (CollObject.gameObject.transform.position.y < objectRb.transform.position.y)
    {
        StartCoroutine(DisplayQuickMessage(PlayerStatus,"Player 1 Wins the Collision",0.3f));
    }
    else if (CollObject.gameObject.transform.position.y > objectRb.transform.position.y)
    {
        score = score - 3;
        SetCountText();
        StartCoroutine(DisplayQuickMessage(PlayerStatus,"Player 1 Loses the Collision -3",0.3f));
    }
 }
}
...
{% endhighlight %}

Timer:

Here is the script which implements the timer.

{% highlight C# linenos %}
...
Timer = Timer - Time.deltaTime;
string SecondsStr;
if ((int)Timer == 60)
{
	StartCoroutine(DisplayQuickMessage(Wintext,"1 Minute Left",1.0f));
}
else if ((int)Timer == 30)
{
	StartCoroutine(DisplayQuickMessage(Wintext,"30 Seconds Left",1.0f));
}
else if ((int)Timer == 10) 
{
	StartCoroutine(DisplayQuickMessage(Wintext,"10 Seconds Left",1.0f));
}
int Minutes = (int) System.Math.Floor(Timer / 60.0);
int Seconds = (int) (Timer % 60.0);
...
{% endhighlight %}

Camera Positon:

The Camera is set to always positioned at the middle of the two balls.

{% highlight C# linenos %}
...
difference = (player2.transform.position - player.transform.position) / 2.0f;
center = difference + player.transform.position;
positioncenter = center;
positioncenter.y = 5.9f + 1.5f * (float)difference.magnitude; 

transform.position = positioncenter;
transform.LookAt(center);
...
{% endhighlight %}



