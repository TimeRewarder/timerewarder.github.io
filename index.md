This is a poster paper for the 12th International Conference on Learning Representations (ICLR 2024). The [official code](https://github.com/dwjshift/IL_ADS) is available on GitHub.

**Paper Link:**

- Latest version: [arxiv link](https://arxiv.org/abs/2310.07433)
- [Openreview submission](https://openreview.net/forum?id=pPJTQYOpNI)

This paper focuses on the problem known as **Imitation Learning from Observation (ILfO)**, where robots learn by imitating the expert's demonstration video without access to expert's action. Common approaches convert ILfO problems into Inverse Reinforcement Learning (RL) problems, using a proxy reward computed from the agent's and the expert's observations. However, we observe that the tasks with **progress dependency** property pose significant challenges for such approach; in these tasks, the agent needs to initially learn the expert's preceding behaviors before mastering the subsequent ones. We reveals the underlying mechanism of this phenomenon and present a novel **Automatic Discount Scheduling (ADS)** method to address the issue.

<!--
This paper focuses on the challenge known as **Imitation Learning from Observation (ILfO)**, where robots learn by imitating the expert without access to its action. A common ILfO approach is to use a proxy reward computed from the agent's and the expert's observations. We identify that tasks with **progress dependency** property pose significant challenges for such approach, because reward signals assigned to later steps hinder the learning of initial behaviors. To address this issue, we present an **Automatic Discount Scheduling (ADS)** mechanism that adaptively alters the discount factor during the training phase, prioritizing earlier rewards initially and gradually engaging later rewards only when the earlier behaviors have been mastered. 
-->








## When and Why Previous Proxy-Reward-Based Methods Fail?
Consider the following *basketball* task. If we experiment with a simplified setting which only instructs the agent to learn the expert's early behaviors (reaching for and grasping the ball), the agent quickly acquires these skills. However, when tasked with learning the entire expert demonstration, the same method fails to acquire the initial grasping skill and instead moves the empty gripper directly to the basket. This is an intriguing finding: *rewarding later steps in a trajectory seems to negatively impact the agent’s ability to learn the earlier behaviors.*
![motivation](index.assets/motivation.jpg)

We observe a similar phenomenon in many manipulation tasks characterized by **progress dependencies**. In these tasks, the agents trained by previous proxy-reward-based ILfO approaches often fail to mimic the expert's early behaviors. Instead, agents resort to optimizing rewards in later stages by moving to states that appear similar to demonstrated states. These locally optimal but incorrect solutions can hinder the agent's exploration of earlier critical behaviors. (See Section 3 in our paper.)

<!--
Challenges on ILfO with proxy reward:  before earlier behaviors are learned, proxy rewards of later steps negatively impact the agent.

![ab](index.assets/ab.png)

> Can explore trajectories like (b), but fails to learn grasping. This is because [Value for lifting the ball (b)] << [Value for pushing it away (a)].
-->

## Method
To address the above issue, we propose to restrict the impact of later rewards until the agent has mastered the previous behavior. Our **Automatic Discount Scheduling** method continuously monitors the agent’s learning progress and dynamically assigns a discount factor that positively correlates with the progress.

<!--
For tasks with progress dependency, we restrict the impact of later rewards until the agent has mastered the previous behaviors by adaptively alters the discount factor.
-->
![adaptive](index.assets/adaptive.jpg)

## Results

Comparison against start-of-the-art ILfO baselines on 9 challenging Meta-World tasks, with 8 random seeds:

​	![performance](index.assets/result.jpg)


Here are some examples of the policies learned by different methods:

<video src="index.assets/example.mp4" controls autoplay loop width="100%"></video>

## Citation

```
@article{liu2023imitation,
  title={Imitation Learning from Observation with Automatic Discount Scheduling},
  author={Liu, Yuyang and Dong, Weijun and Hu, Yingdong and Wen, Chuan and Yin, Zhao-Heng and Zhang, Chongjie and Gao, Yang},
  journal={arXiv preprint arXiv:2310.07433},
  year={2023}
}
```

