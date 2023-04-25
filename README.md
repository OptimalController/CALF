# Critic As a Lyapunov Function: A Reinforcement Learning approach with Guaranteed Environment Stability

This repository contains the code for reproducing the experiments for the paper "Critic As a Lyapunov Function: A Reinforcement Learning approach with Guaranteed Environment Stability". 

## Table of contents

* **[Setting the environment](#setting-the-environment)**
  * [Installing pyenv](#installing-pyenv)
  * [Manual creation of virtual environments](#manual-creation-of-virtual-python-environment) 
* **[Run experiments](#run-experiments)**
  * [SARSA](#sarsa)
  * [DQN](#dqn)
  * [SDPG](#sdpg)
  * [DDPG](#ddpg)
  * [RPO](#rpo)
  * [CALF Stabilizng Policy](#calf-stabilizing-policy)
  * [CALF](#calf)
  * [MPC with horizon 2](#mpc-with-horizon-2)
  * [MPC with horizon 5](#mpc-with-horizon-5)
  * [MPC with horizon 8](#mpc-with-horizon-8)
    

## Setting the environment

Given code was developed and tested with Python version 3.9.16 on Ubuntu 20/22, we strongly advise to perform all the experiments 
with this specified python version.

It is reasonable to run experiments in virtual environment. Our core team uses [pyenv](https://github.com/pyenv/pyenv) for 
managing the virtual environments. We provide [brief guide](#installing-pyenv) here how to install it. But we strongly recommend to 
refer to original [readme](https://github.com/pyenv/pyenv/#readme) for details. However, there is 
[another way](#manual-creation-of-virtual-python-environment) to create virtual environment. You can use either you want.

### Installing pyenv 

The tutorial is the summary of [original pyenv readme](https://github.com/pyenv/pyenv/#readme) and works on Ubuntu.

1. Install dependiencies
```
sudo apt install build-essential libssl-dev zlib1g-dev \
	libbz2-dev libreadline-dev libsqlite3-dev curl \
	libncursesw5-dev xz-utils tk-dev libxml2-dev libxmlsec1-dev libffi-dev liblzma-dev
```
2. Run `curl https://pyenv.run | bash`

3. Execute the following command if you use [zsh](https://github.com/ohmyzsh/ohmyzsh/wiki/Installing-ZSH) shell.
  ```
	echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
	echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
	echo 'eval "$(pyenv init -)"' >> ~/.zshrc
	exec zsh
  ```
  Or if you have simple bash terminal execute
  ```
  echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.bashrc
  echo 'command -v pyenv >/dev/null || export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.bashrc
  echo 'eval "$(pyenv init -)"' >> ~/.bashrc
  ```
  We stronly recommend to refer to [pyenv readme](https://github.com/pyenv/pyenv/#set-up-your-shell-environment-for-pyenv)
  for details.

4. Restart your terminal

5. `cd` to the root of the repo

6. Run `pyenv install 3.9.16` to install the specific python version.

7. Run `pyenv virtualenv 3.9.16 env-name-you-want`

8. Run `pyenv local env-name-you-want`

### Manual creation of virtual environments

If you don't have python3.9-venv please install it via
```
    sudo apt install python3.9-venv
```
Then create the environment via
```
    python3.9 -m venv env
    source env/bin/activate
```

## Run experiments

In the root of repo before running experiments set environment variables
```
pip install -r requirements.txt --no-cache-dir 
cd playground
```

Below we present how to run algorithms in manuscript on all the environmens.
For every run the code generates the specific folder, where all the 
run artifacts are stored (observations, total objectives, etc.). 
This folder will be generated in `playground/multirun`. 

<!---
**Note**  
We use [hydra](https://github.com/facebookresearch/hydra) for working with configuration setups.

### Systems mapping

In our configuration files we used specific names for all the systems in manuscript.

We provide detailed table here.

| **system naming in manuscript** | **system naming in configs** |
|---------------------------------|------------------------------|
| Two-tank                        | `2tank`                      |
| Three-wheeled robot             | `3wrobot_ni`                 |
| Inverted Pendulum               | `inv_pendulum`               |
| Kinematic Point                 | `kin_point`                  |
| Cartpole                        | `cartpole`                   |
| Lunar Lander                    | `lunar_lander`               |
-->


#### SARSA
  
```
PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=sarsa system=2tank +seed=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=sarsa system=3wrobot_ni +seed=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=sarsa system=inv_pendulum +seed=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=sarsa system=kin_point +seed=6

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=sarsa system=cartpole +seed=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=sarsa system=lunar_lander +seed=5
```

#### DQN

```
PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=dqn system=2tank +seed=4

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=dqn system=3wrobot_ni +seed=11

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=dqn system=inv_pendulum +seed=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=dqn system=kin_point +seed=6

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=dqn system=cartpole +seed=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=dqn system=lunar_lander +seed=3
```

#### SDPG

```
PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=acpg system=2tank +seed=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=acpg system=3wrobot_ni scenario=episodic_reinforce +seed=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=acpg system=inv_pendulum scenario=episodic_reinforce +seed=4

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py scenario=episodic_reinforce controller/actor/model=acpg_kin_point_elem_wise controller=acpg system=kin_point +seed=2

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py scenario=episodic_reinforce controller=acpg system=cartpole +seed=4

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=acpg system=lunar_lander scenario=episodic_reinforce +seed=1
```
### DDPG

```
PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=ddpg system=2tank scenario=episodic_reinforce +seed=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py scenario=episodic_reinforce controller=ddpg system=3wrobot_ni +seed=18

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=ddpg system=inv_pendulum scenario=episodic_reinforce +seed=3

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=ddpg system=kin_point scenario=episodic_reinforce +seed=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=ddpg system=cartpole scenario=episodic_reinforce +seed=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=ddpg system=lunar_lander scenario=episodic_reinforce +seed=1
```

### RPO
```
PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=rpo system=2tank +seed=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=rpo system=3wrobot_ni +seed=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=rpo system=kin_point +seed=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=rpo system=lunar_lander +seed=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=rpo system=cartpole +seed=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py controller=rpo system=inv_pendulum +seed=1

```

### CALF Stabilizing Policy
```
PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=kin_point controller=calf_ex_post initial_conditions=ic_kin_point_stochastic +controller.safe_only=True +seed=1 scenario.N_episodes=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=inv_pendulum controller=calf_ex_post initial_conditions=ic_inv_pendulum_stochastic +controller.safe_only=True +seed=1 scenario.N_episodes=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=3wrobot_ni controller=calf_ex_post initial_conditions=ic_3wrobot_ni_stochastic +controller.safe_only=True +seed=1 scenario.N_episodes=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=2tank controller=calf_ex_post initial_conditions=ic_2tank_stochastic +controller.safe_only=True +seed=1 scenario.N_episodes=1

PYTHONPATH=$(pwd)/src-1 python preset_cartpole.py controller=calf_ex_post system=cartpole +controller.safe_only=True +seed=1 scenario.N_episodes=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=lunar_lander controller=calf_ex_post initial_conditions=ic_lunar_lander_stochastic +controller.safe_only=True +seed=1 scenario.N_episodes=1
```

### CALF
```
PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=2tank controller=calf_ex_post initial_conditions=ic_2tank_stochastic +seed=14 controller.actor.predictor.prediction_horizon=0

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=3wrobot_ni controller=calf_predictive initial_conditions=ic_3wrobot_ni_stochastic +seed=1 controller.actor.predictor.prediction_horizon=0

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=inv_pendulum controller=calf_ex_post initial_conditions=ic_inv_pendulum_stochastic +seed=2 controller.actor.predictor.prediction_horizon=0

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=kin_point controller=calf_predictive initial_conditions=ic_kin_point_stochastic +seed=1 controller.actor.predictor.prediction_horizon=0 

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=lunar_lander controller=calf_ex_post initial_conditions=ic_lunar_lander_stochastic +seed=1 controller.actor.predictor.prediction_horizon=0

PYTHONPATH=$(pwd)/src-1 python preset_cartpole.py system=cartpole controller=calf_ex_post 
```

### MPC with horizon 2
```
PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=2tank controller=mpc +seed=1 controller.actor.predictor.prediction_horizon=2 scenario.N_episodes=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=3wrobot_ni controller=mpc +seed=1 controller.actor.predictor.prediction_horizon=2 scenario.N_episodes=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=inv_pendulum controller=mpc +seed=1 controller.actor.predictor.prediction_horizon=2 scenario.N_episodes=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=kin_point controller=mpc +seed=1 controller.actor.predictor.prediction_horizon=2 scenario.N_episodes=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=lunar_lander controller=mpc +seed=1 controller.actor.predictor.prediction_horizon=2 scenario.N_episodes=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=cartpole controller=mpc +seed=1 controller.actor.predictor.prediction_horizon=2 scenario.N_episodes=1
```

### MPC with horizon 5
```
PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=2tank controller=mpc +seed=1 controller.actor.predictor.prediction_horizon=5 scenario.N_episodes=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=3wrobot_ni controller=mpc +seed=1 controller.actor.predictor.prediction_horizon=5 scenario.N_episodes=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=inv_pendulum controller=mpc +seed=1 controller.actor.predictor.prediction_horizon=5 scenario.N_episodes=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=kin_point controller=mpc +seed=1 controller.actor.predictor.prediction_horizon=5 scenario.N_episodes=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=lunar_lander controller=mpc +seed=1 controller.actor.predictor.prediction_horizon=5 scenario.N_episodes=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=cartpole controller=mpc +seed=1 controller.actor.predictor.prediction_horizon=5 scenario.N_episodes=1
```

### MPC with horizon 8
```
PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=2tank controller=mpc +seed=1 controller.actor.predictor.prediction_horizon=8 scenario.N_episodes=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=3wrobot_ni controller=mpc +seed=1 controller.actor.predictor.prediction_horizon=8 scenario.N_episodes=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=inv_pendulum controller=mpc +seed=1 controller.actor.predictor.prediction_horizon=8 scenario.N_episodes=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=kin_point controller=mpc +seed=1 controller.actor.predictor.prediction_horizon=8 scenario.N_episodes=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=lunar_lander controller=mpc +seed=1 controller.actor.predictor.prediction_horizon=8 scenario.N_episodes=1

PYTHONPATH=$(pwd)/src-2 python preset_endpoint.py system=cartpole controller=mpc +seed=1 controller.actor.predictor.prediction_horizon=8 scenario.N_episodes=1
```