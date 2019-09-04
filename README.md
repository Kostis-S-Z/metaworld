# Meta-World
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](https://github.com/tianheyu927/metaworld/blob/master/LICENSE)

__Meta-World is an open-source simulated benchmark for meta-reinforcement learning and multi-task learning consisting of 50 distinct robotic manipulation tasks.__ We aim to provide task distributions that are sufficiently board to evaluate meta-RL algorithms' generalization ability to new behaviors.

For more information, please refer to our [website](corl2019metaworld.github.io).

__Table of Contents of This Document__
* [Installation](#installation)
* [Using the benchmark](#using-the-benchmark)
  * [Basics](#basics)
  * [Running ML1](#running-ml1)
  * [Running ML10, ML45](#running-ml10-and-ml45)
  * [Running MT10, MT50](#running-mt10-and-mt50)
  * [Running Single Task Environments](#running-single-task-environments)
* [Contributors and Acknowledgement](#contributors-and-acknowledgement)
* [Contributing to Meta-World](#contributing-to-meta-world)

## Installation
Meta-World is based on MuJoCo, which has a proprietary dependency we can't set up for you. Please follow the [instructions](https://github.com/openai/mujoco-py#install-mujoco) in the mujoco-py package for help. Once you're ready to install everything, clone this repository and install:

```
git clone https://github.com/tianheyu927/metaworld.git
cd metaworld
pip install -e .
```

## Using the benchmark
Here is a list of benchmark environments for meta-RL (ML*) and multitask-RL (MT*):
* __ML1__ is a meta-RL benchmark environment to test few-shot adaptation to goal variation within one task. You can choose the task from [50 available tasks](#).
* __ML10__ is a meta-RL benchmark environment to test few-shot adaptation to [new tasks](#) with [10 meta-train tasks](#).
* __ML45__ is a meta-RL benchmark environment to test few-shot adaptation to [new tasks](#) with [45 meta-train tasks](#).
* __MT10__, __MT50__ are a multitask-RL benchmark environments for learning a multi-task policy that generalize to [10]() and [50 training tasks](#). The observation of MT10 and MT50 is augmented with an one-hot vector to provide information of task identities.


### Basics
We provide two extra API's to extend a [`gym.Env`](https://github.com/openai/gym/blob/c33cfd8b2cc8cac6c346bc2182cd568ef33b8821/gym/core.py#L8) interface for meta-RL and multitask-RL:
* sample_tasks(self, meta_batch_size): Return a list of tasks with a length of `meta_batch_size`.
* set_task(self, task): Set the task of a multitask environment.


### Running ML1
```
from metaworld.benchmarks import ML1


print(ML1.available_tasks())  # Check out the available tasks

env = ML1.get_train_tasks('pick_place')  # Create an environment with task `pick_place`
tasks = env.sample_tasks(1)  # Sample a task (in this case, a goal variation)
env.set_task(tasks[0])  # Set task

obs = env.reset()  # Reset environment
a = env.action_space.sample()  # Sample an acion
obs, reward, done, info = env.step(a)  # Step the environoment with the sampled random action
```
or, alternatively you can also create an environment like this:
```
env = ML1(task_name='pick_place', env_type='train')
```

### Running ML10 and ML45
Create an environment with train tasks:
```
from metaworld.benchmarks import ML10
ml10_train_env = ML10.get_train_tasks()
```
or
```
ml10_train_env = ML10(env_type='train')
```
Create an environment with test tasks:
```
ml10_test_env = ML10.get_test_tasks()
```
or
```
ml10_test_env = ML10(env_type='test')
```

### Running MT10 and MT50
Create an environment with train tasks:
```
from metaworld.benchmarks import MT10
mt10_train_env = MT10.get_train_tasks()
```
or
```
mt10_train_env = MT10(env_type='train')
```
Create an environment with test tasks (noted that the train tasks and tests task for multitask (MT) environments are the same):
```
mt10_test_env = MT10.get_test_tasks()
```
or
```
mt10_test_env = MT10(env_type='test')
```

### Running Single Task Environments
Meta-World can also be used as a normal `gym.Env` for single task benchmarking. Here is an example of creating a `pick_place` environoment:
```
from metaworld.envs.mujoco.sawyer_xyz import SawyerReachPushPickPlace6DOFEnv
env = SawyerReachPushPickPlace6DOFEnv()
```

## Contributors and Acknowledgement
Meta-World is a work by [Tianhe Yu (Stanford University)](https://cs.stanford.edu/~tianheyu/), [Deirdre Quillen (UC Berkeley)](https://scholar.google.com/citations?user=eDQsOFMAAAAJ&hl=en), [Zhanpeng He (Columbia University)](https://zhanpenghe.github.io), [Ryan Julian (University of Southern California)](https://robotics.usc.edu/resl/people/89/), [Karol Hausman (Google AI)](https://karolhausman.github.io), [Sergey Levine (UC Berkeley)](https://people.eecs.berkeley.edu/~svlevine/) and [Chelsea Finn (Stanford University)](https://ai.stanford.edu/~cbfinn/).

If you use Meta-World for your academic research, please kindly cite Meta-World with the following BibTeX:

```
@misc{yu2018,
  Author = {Tianhe Yu and Deirdre Quillen and Zhanpeng He and Ryan Julian and Karol Hausman and Sergey Levine and Chelsea Finn},
  Title = {Meta-World: A Benchmark and Evaluation for Multi-Task and Meta-Reinforcement Learning},
  Year = {2019},
  url = "https://corl2019metaworld.github.io/"
}
```
Meta-World is originally based on [multiworld](https://github.com/vitchyr/multiworld), which is developed by [Vitchyr H. Pong](https://people.eecs.berkeley.edu/~vitchyr/), Russell Mendonca and [Ashvin Nair](http://ashvin.me/). The Meta-World authors are grateful for their efforts on providing such a great framework as a foundation of our work.

## Contributing to Meta-World
We welcome all contributions to Meta-World. Please refer to our [contribution document]() as a guideline for contributing.
