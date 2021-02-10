---
layout: page
usemathjax: true
title: "Introduction"
main_title: "Learning Geometry and Spatial Sense from Vision for Robotic Manipulation"
permalink: /robot-learning-geometry/introduction/
---
<div style="text-align:center;">
<a href="{{ BASE_PATH }}/robot-learning-geometry/" >Cover Page</a> &nbsp;| &nbsp;  <a class="active_href" href="{{ BASE_PATH }}/robot-learning-geometry/introduction/">Introduction</a> &nbsp; | &nbsp;  <a href="{{ BASE_PATH }}/robot-learning-geometry/tutorial">Tutorial</a>&nbsp;| &nbsp;  <a href="{{ BASE_PATH }}/robot-learning-geometry/proposal">Proposal</a>
</div>
<h6 class='page-author'>Jun Jin <br/> Robotics & Vision Lab, Computing Science, University of Alberta</h6>


<br>

> ''Learn how to see. Realize that everything connects to everything else." <br> --- Leonardo da Vinci

<br>

## 1. Introduction
One characteristic of robotic task learning that differs from computer vision or natural language processing, 
is that defining _"what is the task"_ per se is typically more difficult than task objectives like object detection or text classification.

In fact, we have very limited options in our toolbox to define "_what_", 
including task function in early works [[1]](#c1)[[2]](#c2), reward function in reinforcement learning [[3]](#c3)[[4]](#c4), 
behavior cloning in imitation learning [[5]](#c5), 
and hierarchical approaches like inferring task goals in zero-shot imitation learning [[6]](#c6), task planning in
neural task programmers [[7]](#c7) and so on [[8]](#c8)[[9]](#c9)[[10]](#c10). 
Unfortunately, none of these have a thorough study on their capability to define "_what_", 
nor enough attention has been given to the problem of task specification.

#### __1.1 Robotic Task Representation Learning__

<img src= "{{ BASE_PATH }}/assets/images/overview2.png" alt="overview2" class="center" style="width:90%;"/>

The goal of the research presented is to use structured priors and representation learning to encode _what_ is a task, 
with which a task can be generally parameterized, monitored and controlled. 
This problem, which we call __robotic task representation learning__ that involves studying 
- (1) What structured priors should be introduced to represent a task that enables general task specification.
- (2) How a task-relevant representation should be structured
and learned from raw image inputs.
- (3) How such representation enables easier controller design, provides task monitoring. 
- (4) How representational invariance further enables generalizable robot learning across different environments and categorical objects.

For example, by introducing a geometry structured task prior, 
a screwing task's representation encoding $$Z_{t}$$ stays consistent regardless of types of screw driver or screws since the task can be invariantly 
defined as a relationship between tooltip and the screw top.


<img src= "{{ BASE_PATH }}/assets/images/human_task_specification.png" alt="human_task_specification" class="center"/>

Human empirically has a good sense of "what is the task", as studied in observational learning[[11]](#c11) in psychology, 
the rapid grasp of task concept by simply watching adults or peers during a human infant's cognitive developmental process is astonishing. 
Generally, there are two types of structures for task specification used by humans: 
- (1) A semantic structure that conceptually describes how a task should be organized, for example a recipe defines how to make a pizza. 
- (2) A geometric structure that solidly defines spatial information of a task, for example a tenon assembly task without any nails. 

Similarly, the above two structures are applied in robotic research. The semantic approach is intensively studied in ontology and affordance based methods[[11]](#c11) which describe a task in a tree of 
graph structure based on objects,  attributes and their functionalities. 
However, the mapping from semantic task concepts to robot actions without hand-engineered motion primitives remains challenging. 
The geometric approach[[12]](#c12)[[13]](#c13)[[14]](#c14)[[15]](#c15), as a comparison, defines a task by object spatial relationships, geometric constraints 
(point-coincidence, line-parallel, etc.) and their chaining to construct more complex tasks[[16]](#c16). 
Besides the advantage of task interpretability, it provides a way to monitor task execution and relates directly to 
robot control as studied in visual servoing[[17]](#c17). However, this approach is __rarely studied__.


#### __1.2 Making Sense of Vision by Visual Feature Connections__

<img src= "{{ BASE_PATH }}/assets/images/chapter_1_intro.png" alt="chapter_1_intro" class="center"/>

Using geometry and spatial sense to define a manipulation task, is essentially using a 
predefined structure to make connections between objects and geometric features (object parts), 
in order to make sense of what has been seen. The above figure shows 17 examples of how 
geometry and spatial sense applies in various manipulation, gaming, autonomous driving 
and collision avoidance tasks. 


__The theme of this research__ is to introduce geometry and spatial sense as a structured task representation in robot learning, for the purpose of improving 
state-of-art robotic task specification capabilities.

## 2 Task Specification by Geometric Constraints

<div>
<img src= "{{ BASE_PATH }}/assets/images/chapter2_real_examples.png" alt="chapter2_real_examples" class="center" style="width:50%;"/> 
<h5 style="font-weight:bold;text-align:center;">Tasks defined using geometric constraints[[15]](#c15).</h5>
</div>

In robotics, representing a task using connection between geometric features/primitives was firstly studied in 
visual servoing[[17]](#c17), where the connection is structured using geometric constraints. Research started with two-point 
coincidence and soon expanded to point-to-line, line-to-line, more geometric features (planes, conics, cylinars)[[12]](#c12) and 
hierarchical structure linking[[16]](#c16) of the geometric constraints. Visual servoing, formed as a closed loop feedback control, 
maps _what_ defined by observed geometric constraints to robot actions since the geometric loss signal directly links to 
camera/robot motion, which is referred as the __virtual linkage__. Introducing geometry and spatial sense as a relational 
inductive bias in robot learning can encode the spatial characteristics of a task. 

<img src= "{{ BASE_PATH }}/assets/images/chapter2_basis.png" alt="chapter2_basis" class="center"/> 
 
However, in conventional visual servoing, specifying a task is manually done by selecting visual features and their relationships, 
and by long-term trackers of each visual feature. There is no learnable structure as of task definition encoding. 
Revisiting the nature of visual servoing, we have the following observation:
<br>

> ''Using such features enables to realize a large variety of robotic tasks. ... In fact, it can be shown that such an ability relies on the knowledge of the spatio-temporal behavior of a visual data with respect to camera motion." <br> --- Chaumette, F. [[12]](#c12)

<br>
, which means all the matters is the spatio-temporal behavior that relates visual observations, task definitions and camera motion in
a unifying way. From a task function perspective, the above observation indicates all that matters is the spatio-temporal structure of the 
associated task function. Such __spatio-temporal structure should be consistently retrieved__ from observed images for the purpose of continuous 
control. The common failures (either lost-tracking or feature outside camera's field of view) of conventional visual servoing can be explained 
by the violations of such consistency. 

We propose to directly learn such spatio-temporal structure of the associated task function. Moreover, since the spatial part of such behavior 
can directly link to camera motions using either virtual linkage as described above or model based learning methods as described in Chapter 2, 
studying how the spatial part relates to the task definition becomes the dominant problem, which we format using __robotic task representation learning__ as learning a 
__task encoder__ that consistently retrieves a __task encoding__ from an observed image given geometry and spatial sense as the __structured priors__.

Intuitively, there are three learning paradigms can be used to learn such task encoding: 

- (1) A supervised learning approach using human annotated samples, but requires intimidating word load for each task which is not practical.
- (2) An reinforcement learning (RL) approach using reward as task definition and geometry as inductive bias in the state representation, but requires numerous robot environment interactions which is not practical either.
- (3) An unsupervised learning approach using 3rd-view human demonstration video performing the task, but requires unsupervised clues to relate the structured task encoding to task definition.

 <img src= "{{ BASE_PATH }}/assets/images/Our_approach.png" alt="Our_approach" class="center" style="width: 90%;"/>

 In this research, we propose a series of unsupervised learning methods to directly learn this task encoding from 3rd view human demonstration videos. Through examples of various manipulation tasks, we prove introducing geometry structure in robot learning will largely reduce needed human demonstrations, improve learning efficiency, demonstrate better generalization performance.




## References
<a id="c1">[1]</a> 
Samson, Claude, Bernard Espiau, and Michel Le Borgne. Robot control: the task function approach. Oxford University Press, Inc., 1991.

<a id="c2">[2]</a> 
Samson, C., and B. Espiau. "Application of the task-function approach to sensor-based control of robot manipulators." IFAC Proceedings Volumes 23.8 (1990): 269-274.

<a id="c3">[3]</a>
Sutton, Richard S., and Andrew G. Barto. Reinforcement learning: An introduction. MIT press, 2018.


<a id="c4">[4]</a>
Levine, Sergey, et al. "End-to-end training of deep visuomotor policies." The Journal of Machine Learning Research 17.1 (2016): 1334-1373.


<a id="c5">[5]</a>
Argall, Brenna D., et al. "A survey of robot learning from demonstration." Robotics and autonomous systems 57.5 (2009): 469-483.


<a id="c6">[6]</a>
Pathak, Deepak, et al. "Zero-shot visual imitation." Proceedings of the IEEE conference on computer vision and pattern recognition workshops. 2018.


<a id="c7">[7]</a>
Xu, Danfei, et al. "Neural task programming: Learning to generalize across hierarchical tasks." 2018 IEEE International Conference on Robotics and Automation (ICRA). IEEE, 2018.

<a id="c8">[8]</a>
Yang, Yezhou, et al. "Robot learning manipulation action plans by" watching" unconstrained videos from the world wide web." Proceedings of the AAAI Conference on Artificial Intelligence. Vol. 29. No. 1. 2015.


<a id="c9">[9]</a>
Xiong, Caiming, et al. "Robot learning with a spatial, temporal, and causal and-or graph." 2016 IEEE International Conference on Robotics and Automation (ICRA). IEEE, 2016.


<a id="c10">[10]</a>
Sieb, Maximilian, et al. "Graph-structured visual imitation." Conference on Robot Learning. PMLR, 2020.


<a id="c11">[11]</a>
Burke, Christopher J., et al. "Neural mechanisms of observational learning." Proceedings of the National Academy of Sciences 107.32 (2010): 14431-14436.


<a id="c12">[12]</a>
Chaumette, François. "Visual servoing using image features defined upon geometrical primitives." Proceedings of 1994 33rd IEEE Conference on Decision and Control. Vol. 4. IEEE, 1994.

<a id="c13">[13]</a>
Dodds, Zachary, et al. "Task specification and monitoring for uncalibrated hand/eye coordination." Proceedings 1999 IEEE International Conference on Robotics and Automation (Cat. No. 99CH36288C). Vol. 2. IEEE, 1999.

<a id="c14">[14]</a>
Hager, Gregory D., and Zachary Dodds. "On specifying and performing visual tasks with qualitative object models." Proceedings 2000 ICRA. Millennium Conference. IEEE International Conference on Robotics and Automation. Symposia Proceedings (Cat. No. 00CH37065). Vol. 1. IEEE, 2000.

<a id="c15">[15]</a>
Hespanha, João Pedro, et al. "What tasks can be performed with an uncalibrated stereo vision system?." International Journal of Computer Vision 35.1 (1999): 65-85.

<a id="c16">[16]</a>
Dodds, Zachary, et al. "A hierarchical vision architecture for robotic manipulation tasks." International Conference on Computer Vision Systems. Springer, Berlin, Heidelberg, 1999.


<a id="c17">[17]</a>
Hutchinson, Seth, Gregory D. Hager, and Peter I. Corke. "A tutorial on visual servo control." IEEE transactions on robotics and automation 12.5 (1996): 651-670.
