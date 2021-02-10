---
layout: page
usemathjax: true
title: "Proposal"
main_title: "Learning Geometry and Spatial Sense from Vision for Robotic Manipulation"
permalink: /robot-learning-geometry/proposal/
---

<div style="text-align:center;">
<a href="{{ BASE_PATH }}/robot-learning-geometry/" >Cover Page</a> &nbsp;| &nbsp;  <a href="{{ BASE_PATH }}/robot-learning-geometry/introduction/">Introduction</a> &nbsp; | &nbsp;  <a href="{{ BASE_PATH }}/robot-learning-geometry/tutorial">Tutorial</a>&nbsp;| &nbsp;  <a class="active_href"  href="{{ BASE_PATH }}/robot-learning-geometry/proposal">Proposal</a>
</div>
<h6 class='page-author'>Jun Jin <br/> Robotics & Vision Lab, Computing Science, University of Alberta</h6>

<!---
![GSD1 phenotype]({{ BASE_PATH }}/assets/images/chapter_1_intro.png)
$$X_{1}^{2}$$
some citations [[1]](#c1)
-->

<br>
## 5. Learning Invariant Task Descriptors (Proposed)
<br>
![chapter5_invariant_geometry_small]({{ BASE_PATH }}/assets/images/chapter5_invariant_geometry_small.png)


In this section, we keep exploring how a geometry-structured task encoding $$Z$$ will __generalize across tasks with categorical objects__. The term "categorical" 
means objects share the same functional parts that relate to a task definition. For example, in an open-bottle-cap task, the cap of various bottles is the shared functional parts,
and the task definition stays the same by relating gripper fingers to the caps. What insights can this bring to us?

#### __5.1  Task Specification Invariance__

Commonly, a task has two parts: a _"what"_ specifies the task and a _"how"_ drives task execution. 
One interesting characteristic of a manipulation tasks’s _"what"_ is its consistency in the time domain that it won’t change during task execution, 
consistency across different task executors that it won’t change from human to robot, consistency in object categories that it won’t change because of different textured objects, 
consistency in different environmental settings that it won't change given different backgrounds, illuminations and initial conditions. We call it as __"Task Specification Invariance"__.

<img src= "{{ BASE_PATH }}/assets/images/chapter5_real_tasks.png" alt="chapter5_real_tasks" class="center" style="width:98%;"/>

When looking from a geometry perspective, it means that the relational parts of objects form an invariant task 
representation. For example (A), the screw driver's functional part is always a pen tip regardless of what size, color 
of a screw driver it is. The screwing task is always the relationship between its pen tip to the screw top, 
and its vertical direction to the screw body. For another example (B), the book organizing task needs always look at the relationship
of a book edge to the vertical line of book piles, regardless of what kind of book (size, texture) it is. 
To this end, readers probably realized the relationship in geometry feature level between object-robot gripper, 
object-object is the key for manipulation task representation.

#### __5.2  Approach: Expert Orderness Assumption + Task Representation Consistency__

As a result, representing a manipulation task using relational parts of the objects brings the insight of invariant task representation 
which is crucial for robot learning in order to generalize well. We propose to introduce geometry as inductive bias to learn an invariant task representations.
Specifically, given 3rd-view human demonstration videos performing the same task with different objects,  we aim to optimize a graph structured encoder and selector
that selects connections between functional parts of objects, as an invariant task representation.

<img src= "{{ BASE_PATH }}/assets/images/chapter5_optimization.png" alt="chapter5_optimization" class="center"/>

We propose an approach utilizing the __"Expert Orderness Assumption on Categorial Objects"__ defined as: Given several human demonstration videos of performing 
the same task but with categorical objects:
- (1) the temporal order of frame transitions of each video near-optimally defines the task, 
- (2) the task descriptor $$\mathbf{Z}^{\mathbb{G}}_{i}$$ of each video stays consistent, where $$\mathbb{G}$$ defines the prior geometry structure.

#### __5.3  Evaluation__

![sapien]({{ BASE_PATH }}/assets/images/sapien.png)

To evaluate our proposed method, we design a table organization task in a Sapien simulator[[1]](#c1). The task is to organize messy items on a table in a neat way. It was 
firstly introduced in IROS 2020 as a manipulation challenge called "Open Cloud Robot Table Organization Challenge" (OCRTOC)[[2]](#c2). In OCRTOC, an organization task is defined by specifying 
each object's target pose w.r.t. the table, which is quite cumbersome. In this work, we change it to a more natural setting: human demonstrate the organization task using different object
instances, robot is requried to learn the task that generalizes to categorical objects. Under this new setting, the robot is required to organize books, mugs in a neat way 
regardless of what books or mugs it is manipulating. As a result, it's suitable for the study of categorical object generalization.

#### __5.4  Summary__

We aim to investigate the following research questions:
- __Methodology__: 
  -  Given a set of 3rd-view human demonstration videos of different objects, how should we learn an invariant task descriptor?
  -  How does a geometry structured task priori $$\mathbb{G}$$ helps compared to directly learn from image pixels?
- __Controllability__: Does the output geometric loss provide controllability?
  -  In a calibrated scenario, does the output directly apply in a visual servoing controller?
  -  In an uncalibrated scenario, does the output enable efficient learning of a controller using reinforcement learning?
- __Applicability and Generalization__: What kind of tasks does the above method work? 
  -  What kind of geometry structured task priori $$\mathbb{G}$$ does it work? For example, does it work for a line-to-line task? How about a point-to-line task, etc.?
  -  What categorical objects does it work? What's the definition of categorical objects? For example, does it work for different shaped cups but with low image textures? Does it work for same shaped books but with hugely different textures?
    


## References

<a id="c1">[1]</a>
UCSD. Sapien project, "https://sapien.ucsd.edu/", 2020.

<a id="c2">[2]</a>
OCRTOC. Open cloud robot table organization challenge (ocrtoc), "http://www.ocrtoc.org/", 2020.
