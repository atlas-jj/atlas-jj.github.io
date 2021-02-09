---
layout: page
usemathjax: true
title: "Robot Learning Geometry"
main_title: "Learning Geometry and Spatial Sense from Vision for Robotic Manipulation (Proposal)"
permalink: /robot-learning-geometry/
---

<h6 class='page-author'>Jun Jin &nbsp;   &nbsp;   &nbsp;   &nbsp;   &nbsp;  Feb 8, 2021</h6>

<!---
![GSD1 phenotype]({{ BASE_PATH }}/assets/images/chapter_1_intro.png)
$$X_{1}^{2}$$
some citations [[1]](#c1)
-->
<br/>
![GSD1 phenotype]({{ BASE_PATH }}/assets/images/cover_image.png)


<br>

> ''Learn how to see. Realize that everything connects to everything else." <br> --- Leonardo da Vinci

<br>

## 1. Introduction

One characteristic of robotic task learning that differs from learning tasks in computer vision (CV) or 
natural language processing (NLP), is that defining "_what is the task_" per se is typically more 
difficult than task objectives like object detection or text classification.


In fact, we have very limited options in our toolbox to define "_what_", 
including task function in early works [[1]](#c1)[[2]](#c2), reward function in reinforcement learning [[3]](#c3)[[4]](#c4), 
behavior cloning in imitation learning [[5]](#c5), 
and hierarchical approaches like inferring task goals in zero-shot imitation learning [[6]](#c6), task planning in
neural task programmers [[7]](#c7) and so on [[8]](#c8)[[9]](#c9)[[10]](#c10). 
Unfortunately, none of these have a thorough study on their capability to define "_what_", 
nor enough attention has been given to the problem of task specification.


<img src= "{{ BASE_PATH }}/assets/images/overview2.png" alt="overview2" class="center" style="width:90%;"/>

The goal of the research presented is to use structured priors and representation learning to encode _what_ is a task, 
with which a task can be generally parameterized, monitored and controlled. 
This problem, which we call __robotic task representation learning__, involves studying 
- (1) how a task-relevant representation should be structured
and learned from raw image inputs, and
- (2) how such representation enables easier controller design, provides task monitoring and 
- (3) how representational invariance further enables generalizable robot learning across different environments and categorical objects. 

For example, by introducing a geometry structured task prior, 
a screwing task's representation encoding $$Z_{t}$$ stays consistent regardless of types of screw driver or screws since the task can be invariantly 
defined as a relationship between tooltip and the screw top.


<img src= "{{ BASE_PATH }}/assets/images/human_task_specification.png" alt="human_task_specification" class="center"/>

Human empirically has a good sense of "what is the task", as studied in observational learning[[20]](#c20) in psychology, 
the rapid grasp of task concept by simply watching adults or peers during a human infant's cognitive developmental process is astonishing. 
Generally, there are two types of structures for task specification used by humans: 
- (1) A semantic structure that conceptually describes how a task should be organized, for example a recipe defines how to make a pizza. 
- (2) A geometric structure that solidly defines spatial information of a task, for example a tenon assembly task without any nails. 

Similarly, the above two structures are applied in robotic research. The semantic approach is intensively studied in ontology and affordance based methods[[11]](#c11) which describe a task in a tree of 
graph structure based on objects,  attributes and their functionalities. 
However, the mapping from semantic task concepts to robot actions without hand-engineered motion primitives remains challenging. 
The geometric approach[[12]](#c12)[[13]](#c13), as a comparison, defines a task by object spatial relationships, geometric constraints 
(point-coincidence, line-parallel, etc.) and their chaining to construct more complex tasks[[16]](#c16). 
Besides the advantage of task interpretability, it provides a way to monitor task execution and relates directly to 
robot control as studied in visual servoing[[19]](#c19). However, this approach is __rarely studied__.


#### __Making Sense of Vision by Visual Feature Connections__

<img src= "{{ BASE_PATH }}/assets/images/chapter_1_intro.png" alt="chapter_1_intro" class="center"/>

Using geometry and spatial sense to define a manipulation task, is essentially using a 
predefined structure to make connections between objects and geometric features (object parts), 
in order to make sense of what has been seen. The above figure shows 17 examples of how 
geometry and spatial sense applies in various manipulation, gaming, autonomous driving 
and collision avoidance tasks. 


__The theme of this research__ is to introduce geometry and spatial sense as a structured task representation in robot learning, for the purpose of improving 
state-of-art robotic task specification capabilities.

## 2. Learning Geometry and Spatial Sense from Vision

So far, we've discussed general ideas of representing a robotic task based on geometry and spatial sense, which are essentially making feature connections as discussed above.
To ground our discussion, three questions arise:
- (1): how to parameterize geometry and spatial sense in an end-to-end trainable manner that is scalable to encode any forms of visual feature connections; 
- (2): how to relate such parameterization to a task definition in an unsupervised manner, without human annotated samples; 
- (3): what benefits will such structured task representation bring to us, regarding learning efficiency, generalization, intepretability and so on. 


<img src= "{{ BASE_PATH }}/assets/images/Our_approach.png" alt="Our_approach" class="center" style="width: 90%;"/>
In this research, we propose a series of unsupervised learning methods to directly learn task 
representation from 3rd-view human demonstration videos. Through examples of various manipulation tasks, we prove introducing geometry structure in robot learning will largely
reduce needed human demonstrations, improving learning efficiency, showing better generalization performance, and the learned geometric task repressentation provides decidability,
diagnosability and controllability.

<br/>

## 3. Learning Object Level Spatial Sense

<img src= "{{ BASE_PATH }}/assets/images/chapter3_overview.png" alt="chapter3_overview" class="center"/>

Let's begin by exploring how to learn object-level spatial sense from 3rd-view human demonstration videos. This work has been published as
<a href="https://ieeexplore.ieee.org/document/8793649" target="_blank">_"Robot Eye-hand Coordination Learning by Watching Human Demonstrations: a Task Function Approach"_</a>, which appeared as a conference paper in 
_"2019 IEEE International Conference on Robotics and Automation (ICRA)"_.


The basic idea is quite simple: since the most significant source of observed pixel changes $$(S_{t} \rightarrow S_{t+1})$$ is the spatial information variants
caused by human or robot action, is it possible to __retrieve the spatial information from such a state change tuple $$(S_{t}, S_{t+1})$$__?

![chapter3_encoder_orderness]({{ BASE_PATH }}/assets/images/chapter3_encoding_orderness.png)

Suppose we observe an image state $$S_{t}$$ at time t and apply action $$a_{t}$$ which leads to the next image state $$S_{t+1}$$. 
Suppose a state change tuple is parameterized as $$ ds_{t} = f(S_{t}, S_{t+1}) $$, where $$f$$ can be a simple two-state subtraction or a 
neural network. Let $$\mathbf{R_{t}} \in \mathbb{R}^{d}$$ denote the spatial information retrieved by a task encoder function defined as:

$$\mathbf{R_{t}} = {E}_{\mathbf{\theta}}(ds_{t}|a_{t})$$

<!---
, where $$\mathbf{\theta}$$ are the parameters of task encoder function, and $$a_{t}$$ is a _generic action_ which defines any actions that will cause state change $$ds_{t}$$ regardless of human or robot specific actions. 
This definition of $$a_{t}$$ circumvents the _correspondence problem_ as studied in learning from demonstration (LfD) literature~\cite{argall2009survey} 
without knowing a specific action format since we directly study motion outcomes across a human demonstrator and a robot imitator. 
Then $$a_{t}$$ is implicitly represented in $$ds_{t}$$ thus can be removed in later optimization. 
--->
Now the problem becomes: how to optimize 
$${E}_{\theta}$$ given a set of 3rd-view human demonstration videos so that its output $$\mathbf{R_{t}}$$ defines the task? 
Our solution is the introduced __"Expert Orderness Assumption"__ that: 
`given a sequence of human demosntration video frames, the temporal order of frame transitions near-optimally defines
the task.` This assumption guides the unsupervised learning from human demonstration videos, which is proposed as _InMaxEntIRL_.
<!---
The term "near-optimally" refers to the fact that not all human demonstrators are perfect expert, which is similar to imperfect expert demonstration issues as studied in
inverse reinforcement learning (IRL) literature. So 
--->

<a href="https://www.youtube.com/watch?v=KXQbUPw4iw0&ab_channel=JunJin" target="_black"><img src= "{{ BASE_PATH }}/assets/images/ICRA2019_video_play.png" class="center"/></a>

So how it works? In the above stacking blocks task, results show that learning a spatial task encoding enables a moderate success rate given only a small number (1~10) of
human demonstration video samples, and exhibits strong generalization performance regarding task/environment changes.


<img src= "{{ BASE_PATH }}/assets/images/chapter3_vis_encoding.png" alt="chapter3_vis_encoding" class="center" style="width:80%;"/>

Lastly, we examine what exactly has the spatial task encoder learned. We visualize the learned spatial task encoding by collecting samples from a human 
kinesthetic teaching process, then visualize the norm of spatial task encoding output $$R_{t}$$ using a color map. The figure above shows the colored sphere indicating
a tendency towards the 3D target.

<img src= "{{ BASE_PATH }}/assets/images/system_overview3.png" alt="system_overview3" class="center" />
The above figure shows a systematical view of how a spatial task encoder is learned and how to apply the output $$R_{t}$$ in a closed-loop controller.


## 4. Learning Geometry Skills

Successively, we explore further from learning object-level spatial sense to a more fine-grained task representation framework---using geometric constraints, 
i.e., connections between geometric features / primitives, which we refer as geometry skills. This approach of robotic task representation was originally studied in 
visual servoing litetratures[[12]](#c12)[[13]](#c13)[[14]](#c14)[[15]](#c15) which rely on human hand selected features. We propose to directlyl learn geometric feature constraints as a task encoding, and based on which, 
to learn an optimal selector instead of a hand engineered one.

<img src= "{{ BASE_PATH }}/assets/images/chapter4_overview_small.png" alt="chapter4_overview_small" class="center"/>

This work has been published as two papers: <a href="https://ieeexplore.ieee.org/document/9196570" target="_blank">
_"Visual Geometric Skill Inference by Watching Human Demonstration"_</a> appeared as a conference paper in 
_"2020 IEEE International Conference on Robotics and Automation (ICRA)"_, and <a href="https://ieeexplore.ieee.org/document/9196570" target="_blank">
_"A Geometric Perspective on Visual Imitation Learning"_</a> in _"2020 IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)"_.


<!---
Reasons are three folds: (1) An optimal feature connection selector can select feature connections dirrectly 
 from $$\mathcal{F}$$, thus avoiding human hand selection and feature tracking; (2)  Encoding feature connections directly can utilize the rich representation power of deep neural networks;
(3) Encoding feature connections directly can be seen as learning a task representation. It provides the possibility to learn invariant task representations that stay consistent during task execution, 
across environmental settings and categorical objects. We will explore that in the next section.
--->



<!---
This approach of manipulation task representation is based on two assumptions:
(1) a task that can be defined by connections between geometric features on robot end feectors, tools and objects; (2) and all the features can be simutaneously observed in the same camera system.
The above assumption, though limits its applicability, can still cover a wide range of manipulation tasks, for example: non-prehensible tasks --- command the manipulator reaching a target
pose or changing object states, and prehensible tasks --- command the manipulator using tools to change object states.
--->

<img src= "{{ BASE_PATH }}/assets/images/chapter4_task_relevance_selector.png" alt="chapter4_task_relevance_selector" class="center"/>

In our approach, feature connections are represented as vectors $$Z_{i}$$ by an encoder function $$E_{k}$$. And a selector $$U_{k}$$ selects
feature connections based on their task-relevance probability, which is computed from $$Z_{i}$$. As a result, it forms a differentiable stream from feature connections
to their task-relevance probability. Now the question is how to parameterize an encoder $$E_{k}$$?

![chapter4_two_graph_types]({{ BASE_PATH }}/assets/images/chapter4_two_graph_types.png)

 $$E_{k}$$ is essentially a relationship encoder. We propose two types of parameterzation methods based on graph neural networks (GCN). The reasons why using a GCN are: 
 (1) it's __permutation-invariant__ that the output $$Z$$ is invariant to input
feature orders; (2) it's __scalable__ that can define complex relationships. How should we optimize the encoder and selector given a human demonstration video?

<img src= "{{ BASE_PATH }}/assets/images/chapter4_InMaxEntIRL.png" alt="chapter4_InMaxEntIRL" class="center"/>

To optimize the encoder $$E_{k}$$ and selector $$U_{k}$$, we continue to use the _"expert orderness assumption"_
with a changed $$r_{t}$$ definition based on geometric loss computed from geometric constraints. 
 <!---
Type __A__ are feature descriptor based graphs which rely on hand crafted feature descriptors. 
However it limits its applicability since efficient feature descriptors for complex geometric primitives (line, conics) are difficult to design. Type __B__ removes this limitation by
using generalized image patch based graphs, which take a fixed dimension of image patches as input and utilize the rich representation power of deep neeural networks. As a results, deep
feature are learned as the encoder being optimized.
--->

<a href="https://www.youtube.com/watch?v=NnZM5ZsKc1s&ab_channel=JunJin" target="_black"><img src= "{{ BASE_PATH }}/assets/images/learning_geometry_cover.png" class="center"/></a>

So how does it work? We show that, instead of learning actions from image pixels, learning a geometry-parameterized task concept provides an explainable and invariant representation across
demonstrator to imitator under various environmental settings. 

<!---
<img src= "{{ BASE_PATH }}/assets/images/chapter4_five_tasks.jpg" alt="chapter4_five_tasks" class="center"/>
<img src= "{{ BASE_PATH }}/assets/images/chapter4_human_robot.png" alt="chapter4_human_robot" class="center"/>
<img src= "{{ BASE_PATH }}/assets/images/chapter4_generalization_results_small-min.png" alt="chapter4_generalization_results_small-min" class="center"/>
[chapter4_generalization_setup]({{ BASE_PATH }}/assets/images/chapter4_generalization_setup.jpg)
[chapter4_generalization_results]({{ BASE_PATH }}/assets/images/chapter4_generalization_results.png)
The table above shows evaluation results under environmental / task setting changes. Compared to a hand-feature selection and tracking (hand-tracking) method,
this approach learns an optimal selector that directly selects feature connections based on their task relevance.
-->

<img src= "{{ BASE_PATH }}/assets/images/fig8_IROS.png" alt="fig8_IROS" class="center"/>

Lastly, we examine what has been optimized given a human demonstration video. We save snapshots of the learned encoder model at three different training stages: S1, S2, and S3 
represent the early, middle and final training stages respectively. Then the same robot video is fed into the three model snapthos and outputs the time-series geometric loss
from the selected top feature connections. To do this, we had the robot perform the task via teleoperation, then record video frames. Results show that we are actually
optimizing the encoder, selector to produce "good" control signals.


## 5. Learning Invariant Task Descriptors

![chapter5_invariant_geometry_small]({{ BASE_PATH }}/assets/images/chapter5_invariant_geometry_small.png)


In this section, we keep exploring how a geometry-structured task encoding $$Z$$ will __generalize across tasks with categorical objects__. The term "categorical" 
means objects share the same functional parts that relate to a task definition. For example, in an open-bottle-cap task, the cap of various bottles is the shared functional parts,
and the task definition stays the same by relating gripper fingers to the caps. What insights can this bring to us?



Commonly, a task has two parts: a _"what"_ specifies the task and a _"how"_ drives task execution. 
One interesting characteristic of a manipulation tasks’s _"what"_ is its consistency in the time domain that it won’t change during task execution, 
consistency across different task executors that it won’t change from human to robot, consistency in object categories that it won’t change because of different textured objects, 
consistency in different environmental settings that it won't change given different backgrounds, illuminations and initial conditions. We call it as __"Task Specification Invariance"__.


<img src= "{{ BASE_PATH }}/assets/images/TD_task_equivalence.png" alt="TD_task_equivalence" class="center"/>

As illustrated above, the task is to land a cart on a slope at specific positions as defined by two point-coincidences. 
Along the time domain when the cart moves, its positions will change, however the task definition specified by two pairs of point connections 
stays consistent (A). Likewise, the background may change to pink(B), the cart may change to orange color(C) or a smaller size(D), 
however the task definition stays invariant.

<img src= "{{ BASE_PATH }}/assets/images/chapter5_real_tasks.png" alt="chapter5_real_tasks" class="center" style="width:90%;"/>

When looking from a geometry perspective, it means that the relational parts of objects form an invariant task 
representation. For example (A), the screw driver's functional part is always a pen tip regardless of what size, color 
of a screw driver it is. The screwing task is always the relationship between its pen tip to the screw top, 
and its vertical direction to the screw body. For another example (B), the book organizing task needs always look at the relationship
of a book edge to the vertical line of book piles, regardless of what kind of book (size, texture) it is. 
To this end, readers probably realized the relationship in geometry feature level between object-robot gripper, 
object-object is the key for manipulation task representation.

As a result, representing a manipulation task using relational parts of the objects brings the insight of invariant task representation 
which is crucial for robot learning in order to generalize well. We propose to introduce geometry as inductive bias to learn an invariant task representations.
Specifically, given 3rd-view human demonstration videos performing the same task with different objects,  we aim to optimize a graph structured encoder and selector
that selects connections between functional parts of objects, as an invariant task representation.

<img src= "{{ BASE_PATH }}/assets/images/chapter5_optimization.png" alt="chapter5_optimization" class="center"/>

We propose an approach utilizing the __"Expert Orderness Assumption on Categorial Objects"__ defined as: Given several human demonstration videos of performing 
the same task but with categorical objects:
- (1) the temporal order of frame transitions of each video near-optimally defines the task, 
- (2) the task descriptor $$\mathbf{Z}^{\mathbb{G}}_{i}$$ of each video stays consistent, where $$\mathbb{G}$$ defines the prior geometry structure.


![sapien]({{ BASE_PATH }}/assets/images/sapien.png)

To evaluate our proposed method, we design a table organization task in a Sapien simulator[[17]](#c17). The task is to organize messy items on a table in a neat way. It was 
firstly introduced in IROS 2020 as a manipulation challenge called "Open Cloud Robot Table Organization Challenge" (OCRTOC)[[18]](#c18). In OCRTOC, an organization task is defined by specifying 
each object's target pose w.r.t. the table, which is quite cumbersome. In this work, we change it to a more natural setting: human demonstrate the organization task using different object
instances, robot is requried to learn the task that generalizes to categorical objects. Under this new setting, the robot is required to organize books, mugs in a neat way 
regardless of what books or mugs it is manipulating. As a result, it's suitable for the study of categorical object generalization.

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
Paulius, David, and Yu Sun. "A survey of knowledge representation in service robotics." Robotics and Autonomous Systems 118 (2019): 13-30.

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
UCSD. Sapien project, "https://sapien.ucsd.edu/", 2020.

<a id="c18">[18]</a>
OCRTOC. Open cloud robot table organization challenge (ocrtoc), "http://www.ocrtoc.org/", 2020.

<a id="c19">[19]</a>
Hutchinson, Seth, Gregory D. Hager, and Peter I. Corke. "A tutorial on visual servo control." IEEE transactions on robotics and automation 12.5 (1996): 651-670.

<a id="c20">[20]</a>
Burke, Christopher J., et al. "Neural mechanisms of observational learning." Proceedings of the National Academy of Sciences 107.32 (2010): 14431-14436.