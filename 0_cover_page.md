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

<img src= "{{ BASE_PATH }}/assets/images/screw_driver_task.png" alt="screw_driver_task" class="center" style="width:70%;"/>

One characteristic of robotic task learning that differs from computer vision or natural language processing, 
is that defining _"what is the task"_ per se is typically more difficult than task objectives like object detection or text classification. 
This challenge comes from the complexity nature of robotic tasks with various settings ranging from household, production line, 
disaster mitigation and etc. Due to such complexity, building a general model to well specify tasks still remains challenging. We are still in lack of
an efficient tool to generally describe, parametrize, monitor, decide and solve manipulation
tasks.


<img src= "{{ BASE_PATH }}/assets/images/tasks_need_geometric_constraints.png" alt="tasks_need_geometric_constraints" class="center"  style="width:70%;"/> 


This research focuses on robotic tasks that can be specified using geometric constraints. In robotics, representing a task using geometric features / primivies
was studied in the literature of visual servoing. Such geometric constraints generally applied in a wide range of manipulation tasks and make
precise manipulation control possible. As shown in the figure above, a screwing task can be specified by points and lines constraint that makes
controller design to fulfil the task. The figure below shows various task examples that can be specified using geometric constraints.

<img src= "{{ BASE_PATH }}/assets/images/generalizables.png" alt="generalizables" class="center"  style="width:70%;"/> 

This research studies the problem of how to select such geometric constraints and how to use them to represent a task. Compared to the prevalent end-to-end learning approaches, this approach provides 
- (1) diagnosability that the geometric loss output from geometric constraints monitors task execution, 
- (2) generalization that representing a task based on high level geometric features provides invariant properties that are possible to make robot learning generalize to different environment settings and categorical objects. As shown in the figure above,
a geometry structured task representation stays invariant regardless of types of screw drivers or screws. Likewise, the openning cap task depends on
point constraints that are invariant to different types of bottles. 
- (3) controllability that the virtual linkage makes model based control and efficient learning possible, as shown in the figure below.


<img src= "{{ BASE_PATH }}/assets/images/learning_good_what.png" alt="learning_good_what" class="center"  style="width:70%;"/> 


