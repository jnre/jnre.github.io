---
layout: post
title: Visual SLAM
date: 2022-11-11 11:01 +0800
catergories: [SLAM, Computer Vision]
tags: [c++, algo]
---

my introduction to slam came when i was in A*star and at first it was such a mouthful to understand. 

the algorithm behind it was so difficult to understand and i was just thrown a book to read. (my boss left shortly after, it wasnt a good experience)

nevertheless, it being my first job i tried to take what i could learn from this experience.

i would say some online resources that helped were reading through the opencv documents, ROS docs and a very helpful friend. a book i would highly
recommend for vision based slam is [https://github.com/gaoxiang12/slambook-en](https://github.com/gaoxiang12/slambook-en). i was tasked to integrate visual based odometry into an exisiting robot using cameras, and hopefully slam to it.

i started playing around with a 2-camera setup(stereo) mount as that would provide depth using triangulation. 

the idea behind vision based odometry is actually pretty simple, it retrieves the post estimate from its previous position based on feature based - in this case ORB-SLAM: [https://github.com/raulmur/ORB_SLAM2](https://github.com/raulmur/ORB_SLAM2)

since its a stereo setup, having feature matching on both cameras allow for definite points to be captured. the 2 points and triangluation allows for depth to be captured.

odometry is then captured by comparing the different pose movement of all the points at the instance, so e.g T=3s compared against T=2s.

A sample of the implementation can be seen here, but this implementation isnt all that accurate as by nature all slam face this issue of noise and since its always back mapping bag to the initial pose.

{% include githubPlayer.html id="20430100/201257043-232c609f-da74-4db4-a2eb-761ce6767bdd.mp4" %} 

Of course the people at ORB-SLAM solved this issue with Bag of words where they record each and every cloud of points and once this position in space is being revisited, it would recorrect itself with all the possible errors such that it would close the loop.

## challengers

of course without any guidance it was just walking without a map (haha) where i am constantly lost. the feature based approach would fail if there would be too few features to correctly estimate the next pose (like a white wall). nevertheless i learnt alot of c++ programming and a deeper understand of path mapping/localization and odometry as a algorithm.