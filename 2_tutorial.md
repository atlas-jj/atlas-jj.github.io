---
layout: page
title: "Tutorial"
main_title: "Learning Geometry and Spatial Sense from Vision for Robotic Manipulation"
permalink: /robot-learning-geometry/tutorial/
---
<div style="text-align:center;">
<a href="{{ BASE_PATH }}/robot-learning-geometry/" >Cover Page</a> &nbsp;| &nbsp;  <a href="{{ BASE_PATH }}/robot-learning-geometry/introduction/">Introduction</a> &nbsp; | &nbsp;  <a href="{{ BASE_PATH }}/robot-learning-geometry/tutorial" class="active_href">Tutorial</a>&nbsp;| &nbsp;  <a href="{{ BASE_PATH }}/robot-learning-geometry/proposal">Proposal</a>
</div>
<h6 class='page-author'>Jun Jin <br/> Robotics & Vision Lab, Computing Science, University of Alberta</h6>



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
visual servoing litetratures[[1]](#c1)[[2]](#c2)[[3]](#c3)[[4]](#c4) which rely on human hand selected features. We propose to directlyl learn geometric feature constraints as a task encoding, and based on which, 
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


## References

<a id="c12">[1]</a>
Chaumette, François. "Visual servoing using image features defined upon geometrical primitives." Proceedings of 1994 33rd IEEE Conference on Decision and Control. Vol. 4. IEEE, 1994.

<a id="c13">[2]</a>
Dodds, Zachary, et al. "Task specification and monitoring for uncalibrated hand/eye coordination." Proceedings 1999 IEEE International Conference on Robotics and Automation (Cat. No. 99CH36288C). Vol. 2. IEEE, 1999.

<a id="c14">[3]</a>
Hager, Gregory D., and Zachary Dodds. "On specifying and performing visual tasks with qualitative object models." Proceedings 2000 ICRA. Millennium Conference. IEEE International Conference on Robotics and Automation. Symposia Proceedings (Cat. No. 00CH37065). Vol. 1. IEEE, 2000.

<a id="c15">[4]</a>
Hespanha, João Pedro, et al. "What tasks can be performed with an uncalibrated stereo vision system?." International Journal of Computer Vision 35.1 (1999): 65-85.
