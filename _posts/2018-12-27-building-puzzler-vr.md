---
id: 8388
title: Building Puzzler VR
date: 2018-12-27T14:37:20+00:00
layout: post
author: davidezordan
# guid: https://medium.com/p/6e21dfab1510
# permalink: /?p=8388
image: /wp-content/uploads/2018/12/Puzzler-VR.jpeg
categories:
  - Mixed Reality
  - Mobile
category: blog
tags:
  - Unity
  - Virtual Reality
---
I’ve recently completed a Udacity Virtual Reality foundations Nanodegree and learned a lot about building and optimising applications for mobile VR, so I decided to enrol in the High Immersion course to get more hands-on practice and starting building experiences using both rotational and positional tracking for devices like Oculus Rift and HTC&nbsp;Vive.

The first project of the course was related to VR design and consisted in exploring all the tasks involved in the creation of a complete application including
<ul>
 	<li>sketching the experience</li>
 	<li>creating Unity scenes and deploying to a mobile&nbsp;device</li>
 	<li>user testing</li>
 	<li>iterating using the feedback received during the previous&nbsp;step</li>
</ul>
To put all in practice, I created a project implementing a variation of the game <a href="https://en.wikipedia.org/wiki/Simon_Says">Simon Says</a> starting from the provided GitHub repository <a href="https://github.com/udacity/VR-Design_Puzzler">Udacity&nbsp;Puzzler</a>

<iframe width="640" height="480" src="https://cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fwww.youtube.com%2Fembed%2FCFDUG6owQcM%3Ffeature%3Doembed&amp;url=http%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DCFDUG6owQcM&amp;image=https%3A%2F%2Fi.ytimg.com%2Fvi%2FCFDUG6owQcM%2Fhqdefault.jpg&amp;key=a19fcc184b9711e1b4764040d3dc5c07&amp;type=text%2Fhtml&amp;schema=youtube" frameborder="0" scrolling="no"><a href="https://medium.com/media/f4ef46a07a86b20b9e458b7ba33f82f8/href">https://medium.com/media/f4ef46a07a86b20b9e458b7ba33f82f8/href</a></iframe>
<h3>Introducing Puzzler</h3>
Before starting to code with Unity, I needed to identify the final goal of the game through a statement of&nbsp;purpose:

<em>Puzzler is a Virtual Reality mobile application for new VR users which challenges them to solve a familiar type of puzzle in a new&nbsp;way.</em>

With that in mind, I identified a <em>persona</em> to keep as a reference while developing the&nbsp;game:
<figure><img src="https://cdn-images-1.medium.com/max/128/1*F8Y9z8hhAssrTRjhjvU8tQ.jpeg" alt=""><figcaption><strong>Gilbert</strong>, 35 — Accountant</figcaption></figure>
<em>Gilbert is an accountant. He is married, has two children and likes relaxing by playing video games and using a Virtual Reality device. Usually, he arrives at home around 8 p.m. and enjoys time with the family until the kids go to bed. Then, Gilbert and his wife watch some TV and chat about what happened during the day. On his free time in the weekends, he likes to take his bike for a quick ride. His favourite quote is “don’t waste time”. He is new to Virtual Reality and prefers mobile devices (ref. photo from </em><a href="https://randomuser.me"><em>https://randomuser.me</em></a><em>).</em>

After describing the game persona, I was ready to start working on the initial sketches.
<h3>Environment sketches</h3>
The game environment is a mysterious castle which the user can explore in&nbsp;VR:
<figure><img src="https://cdn-images-1.medium.com/max/1024/1*lA0Gt2QPzuIVD_NMu7uj3Q.jpeg" alt="">

<figcaption>Puzzler — environment sketch</figcaption></figure>
After selecting the <em>Start</em> button, the user is transported into the castle to play the&nbsp;game:
<figure><img src="https://cdn-images-1.medium.com/max/1024/1*R310BqqetdsWxKI1ACN7wQ.jpeg" alt="">

<figcaption>Puzzler — An insight&nbsp;view</figcaption></figure>
I took as a starting point the environment already provided with the project but I realised that it was missing some details to make it more compelling and exciting to explore in VR, so I added some extra buildings and castles in the sketches:
<figure><img src="https://cdn-images-1.medium.com/max/1024/1*UXuf8W2R1CrrAPgazG2EvA.jpeg" alt="">

<figcaption>Puzzler — enhanced&nbsp;view</figcaption></figure>
Great! I was then ready to start putting together all the pieces and build the Unity&nbsp;project.
<h3>Identifying the right&nbsp;scale</h3>
I fired up Unity and imported the project assets: the first task was to determine the correct scale for the&nbsp;objects.

I deployed the scene to my <em>Oculus Go</em> device and verified the size was similar to my apartment door and adjusted it until I reached the desired&nbsp;result:
<figure><img src="https://cdn-images-1.medium.com/max/1024/1*lMfX82Q59k4Sv32-9HZh3Q.jpeg" alt="">

<figcaption>Scale test</figcaption></figure>
Then I proceeded to create a first version of the internal game environment
<figure><img src="https://cdn-images-1.medium.com/max/1024/1*o2ik0mPCsgvCq0P9HGQXmA.jpeg" alt="">

<figcaption>Internal game environment</figcaption></figure>
After this, it was time to start some user testing to check if I was on the right track! First of all, I needed to identify&nbsp;if:
<ul>
 	<li>the scale was appropriate</li>
 	<li>the dimensions of the room were&nbsp;adequate</li>
</ul>
After two users tested the app using the VR device, it was clear that the scale worked quite well, but some additional modifications were required as the room size needed to be increased. I modified the castle and started adding some lights and the game&nbsp;orbs:
<figure><img src="https://cdn-images-1.medium.com/max/1024/1*A_thzGjPwcwMNW7Ywn8BqQ.jpeg" alt="">

<figcaption>The enlarged environment with lightning and&nbsp;orbs</figcaption></figure>
Great! I performed a new user testing session, and the dimensions and lights had positive feedback.

The users were also able to describe the mood of the environment as being in an old castle with mystery and legendary items, so I decided to move on with the next step: UI creation.
<h3>GUI creation</h3>
At this point, I needed to create two panels permitting the users to start and restart the game and came up with the following:
<figure><img src="https://cdn-images-1.medium.com/max/1024/1*dktgLgLWkOVpCYUCQqql3g.jpeg" alt="">

<figcaption>UI first iteration</figcaption></figure>
Before proceeding further, I needed some additional user testing for verifying I was on the right track and immediately realised there were some modifications to be done as the users confirmed that:
<ul>
 	<li>the text was&nbsp;legible</li>
 	<li>panels were too&nbsp;big</li>
 	<li>black colour didn’t fit well with the environment</li>
</ul>
I performed some changes and ended up with a new design for the&nbsp;UI:
<figure><img src="https://cdn-images-1.medium.com/max/1024/1*0NNcijZTnY-fysLOoUHROg.jpeg" alt="">

<figcaption>Start UI</figcaption></figure>
Then I modified the location of the panels and put it in the start/end positions:
<figure><img src="https://cdn-images-1.medium.com/max/1024/1*85AWfnyEwSqBdwnUTw6EXw.jpeg" alt="">

<figcaption>Initial UI location and speed&nbsp;test</figcaption></figure>
At this stage, I added some game movements using ground raycasting for positioning the user from the start to the playroom: these transitions could cause simulator sickness to some users, so I needed to test if the movement speed was adequate.

After some personal and user testing I noted&nbsp;that:
<ul>
 	<li>it was clear how to activate the start button using&nbsp;gaze</li>
 	<li>the speed of movement was too high and causing some disorientation to the&nbsp;user</li>
</ul>
I modified the game slowing down the pace and continued adding the final game mechanics for selecting the orbs, playing the game and adjusting the external game environment with mountains and a bigger castle (using the assets from <a href="https://assetstore.unity.com/packages/3d/environments/fantasy/mega-fantasy-props-pack-87811">Mega Fantasy Props&nbsp;Pack</a>):
<figure><img src="https://cdn-images-1.medium.com/max/1024/1*eL5hQX7dm4n9vNDJlNVSrg.jpeg" alt="">

<figcaption>Welcome to Puzzler&nbsp;UI</figcaption></figure>
<h3>Breakdown of the&nbsp;project</h3>
At this point, the game functionalities were completed, and I was ready to test the experience end-to-end. After deploying and starting the application using my Oculus Go, the initial UI loaded correctly and, after gazing the start button, I was transported to the mysterious play&nbsp;area:
<figure><img src="https://cdn-images-1.medium.com/max/1024/1*tOWPwkrHgtbaauTq7nMAPw.jpeg" alt="">

<figcaption>Play area</figcaption></figure>
<em>Puzzler</em> played a sequence of 5 orbs that I had to memorise and repeat correctly to win the&nbsp;game:
<figure><img src="https://cdn-images-1.medium.com/max/1024/1*web9cJa9WmtOSN2kwWWg8w.jpeg" alt="">

<figcaption>Final UI when the player&nbsp;wins</figcaption></figure>
And pressing the restart button allowed me to restart the game from the beginning.

<iframe width="640" height="480" src="https://cdn.embedly.com/widgets/media.html?src=https%3A%2F%2Fwww.youtube.com%2Fembed%2FCFDUG6owQcM%3Ffeature%3Doembed&amp;url=http%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DCFDUG6owQcM&amp;image=https%3A%2F%2Fi.ytimg.com%2Fvi%2FCFDUG6owQcM%2Fhqdefault.jpg&amp;key=a19fcc184b9711e1b4764040d3dc5c07&amp;type=text%2Fhtml&amp;schema=youtube" frameborder="0" scrolling="no"><a href="https://medium.com/media/f4ef46a07a86b20b9e458b7ba33f82f8/href">https://medium.com/media/f4ef46a07a86b20b9e458b7ba33f82f8/href</a></iframe>
<h3>Conclusions</h3>
I enjoyed working on Puzzler: the main focus of this project was to use a design workflow for a Virtual Reality application and was clear to me the importance of user testing in these types of apps as there are a lot of challenges compared to “traditional” user interfaces. In particular, simulator sickness and performance optimization is probably the most challenging task to achieve high-quality VR&nbsp;apps.
<h3>Next Steps</h3>
I have published this project on <a href="https://github.com/davidezordan/Puzzler">GitHub</a> to start collecting additional feedback and to add extra games/environments in the future: any input on this work would be very much appreciated.
<h3>Link to additional work</h3>
<strong>A maze</strong> — A VR application using Unity and the Google VR SDK where the user traverses a maze environment using 2D and 3D UI, waypoint based navigation, procedural animation, interactive objects, spatial audio, particle effects and persistent storage of session data. <a href="https://github.com/davidezordan/A-Maze">https://github.com/davidezordan/A-Maze</a>

<strong>Build an Apartment</strong> — Explore a modern apartment in VR with high-performance lighting, custom materials, and animation using Unity. <a href="https://github.com/davidezordan/Build-an-apartment">https://github.com/davidezordan/Build-an-apartment</a>

<strong><a href="../analysing-visual-content-using-hololens-computer-vision-apis-unity-and-the-windows-mixed-reality-toolkit">Analysing visual content using HoloLens, Computer Vision APIs, Unity and the Mixed Reality Toolkit</a> </strong>— A GitHub project for exploring the environment using a mixed reality device and computer vision APIs.