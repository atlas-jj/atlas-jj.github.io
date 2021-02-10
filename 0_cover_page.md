---
layout: page
usemathjax: true
title: "Cover Page"
main_title: "Learning Geometry and Spatial Sense from Vision for Robotic Manipulation"
permalink: /robot-learning-geometry/
---
<div style="text-align:center;">
<a href="{{ BASE_PATH }}/robot-learning-geometry/" class="active_href">Cover Page</a> &nbsp;| &nbsp;  <a href="{{ BASE_PATH }}/robot-learning-geometry/introduction/">Introduction</a> &nbsp; | &nbsp;  <a href="{{ BASE_PATH }}/robot-learning-geometry/tutorial">Tutorial</a>&nbsp;| &nbsp;  <a href="{{ BASE_PATH }}/robot-learning-geometry/proposal">Proposal</a>
</div>
<h6 class='page-author'>Jun Jin <br/> Robotics & Vision Lab, Computing Science, University of Alberta</h6>

<br>



Are there any general solutions to parameterize robotic manipulation tasks? 

Well, there is no absolute answer, but even if a parameterization exists,
it is difficult to find because manipulation tasks are too complex in general---various task setting can range from household,  production line, 
disaster mitigation and so many as you could mention.  

Among the various manipulation tasks, in this research, we are interested in those that can be generally parameterized using geometric constraints. 

<img src= "{{ BASE_PATH }}/assets/images/screw_driver_task.png" alt="screw_driver_task" class="center" style="width:70%;"/>

In robotics, representing a task using connections between geometric features / primitives
was studied in the literature of visual servoing. Such geometric constraints generally applied in a wide range of manipulation tasks and make
precise manipulation control possible. For example, in the figure above, a screwing task can be specified by points and lines constraint that makes
a controller design to fulfil the task. There are a wide range of manipulation tasks that can be represented using such geometric constraints since objects in our surroundings are made from regular 
geometric shapes as shown below.


<img src= "{{ BASE_PATH }}/assets/images/tasks_need_geometric_constraints.png" alt="tasks_need_geometric_constraints" class="center"  style="width:70%;"/> 

So why should we study geometric constraints in the era of robot learning? What new insights can it bring to us? 

<img src= "{{ BASE_PATH }}/assets/images/generalizables.png" alt="generalizables" class="center"  style="width:80%;"/> 
 

__The first insight__ is that, if we see geometric constraints as a way to represent a task, we will get __task specification invariance__ which indicates
the definition of a task stays invariant across different environments or categorical objects.  As shown in the figure below, a geometry structured task representation stays invariant regardless of types of screw drivers or screws. 
 Likewise, the openning cap task depends on point constraints that are invariant to different types of bottles. If we further form it as a robotic task representation learning problem, which means we aim to learn an encoder that takes input of such geometric
constraint, and outputs an encoding as the task representation, the generalization problem turns to learning a consistent task representation problem.

<img src= "{{ BASE_PATH }}/assets/images/generalizables.png" alt="generalizables" class="center"  style="width:80%;"/> 


As a result, our approach is essentially introducing a geometry structured prior to the problem of robotic task learning. This aligns with approaches from S. Levine et al. and
Qin et al. that a spatial softmax operator of keypoint filter layer is applied along the optimization of deep RL training. But this is different with a attentional
selection towards only task relevant features because of our introduced geometry structure prior $$\mathbf{G}$$.


The second insight is that, if the learned task representation is indeed encoding task relevant information, then the controller design should be made easier, which means
learning a "good what" to enable efficient and generalizable "how".
