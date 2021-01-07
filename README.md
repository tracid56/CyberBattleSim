﻿# CyberBattleSim


CyberBattleSim is an experimentation research platform to investigate the interaction
of automated agents operating in a simulated abstract enterprise network environment.
The simulation provides a high-level abstraction of computer networks
and cyber security concepts.
Its Python-based OpenAI Gym interface allows for training of
automated agents using reinforcement learning algorithms.

The simulation environment is parameterized by a fixed network topology
with an associated set of vulnerabilities that attacker agents can utilize
to move laterally in the network.
The goal of the attacker is to take ownership of a portion of the network by exploiting
vulnerabilities that are planted in the computer nodes.
While the attacker attempts to spread throughout the network,
a defender agent watches the network activity and tries to contain the attack.

We provide a basic stochastic defender that detects
and mitigate ongoing attacks based on pre-defined probabilities of success.
Mitigation consists in re-imaging the infected nodes, a process
abstractly modeled as an operation spanning over multiple simulation steps.

To compare performance of the agents we look at two metrics: the number of simulation steps taken to
attain their goal and the cumulative rewards over simulation steps across training epochs.

## Project goals

We view this project as an experimentation platform to conduct research on the interaction of automated agents in abstract simulated network environments. By open sourcing it we hope to encourage the research community investigate how cyber-agents interact and evolve in such network environments.

The simulation we provide is admittedly simplistic, but this has advantages. Its highly abstract nature prohibits direct application to real-world systems thus providing a safeguard against potential nefarious use of automated agents trained with it.
At the same time, its simplicity allows us to focus on specific security aspects we aim to study and quickly experiment with recent machine learning and AI algorithms.

For instance, the current implementation focuses on
the lateral movement cyber-attacks techniques, with the hope to understand how the network topology and configuration affects them. With this goal in mind, we felt that modeling actual network traffic was not necessary. This is just one example of a significant limitation in our system that future contributions might want to address.

On the algorithmic side, we provide some basic agents as starting points but we
would be curious to find out how state-of-the art reinforcement learning algorithms compare to them. We found that the large action space
intrinsic to any computer system is a particular challenge for
Reinforcement Learning, in contrasts to other applications such as video games or robot control. Training agents that can store and retrieve credentials is another challenge we faced when applying RL techniques
where agents typically do not feature internal memory. These are other areas of research where the provided simulation can perhaps be used for benchmarking purpose.

Other areas of interests include the responsible and ethical use of autonomous cyber-security systems:
How to design an enterprise network that gives an intrinsic advantage to defender agents?
How to conduct safe research aimed at defending enterprises against autonomous cyber-attacks
while preventing nefarious use of such technology?

## Documentation

Read the [Quick introduction](/docs/quickintro.md) to the project.

## Build status

| Type | Branch | Status |
| ---  | ------ | ------ |
| CI   | master | [![Build Status](https://mscodehub.visualstudio.com/Asterope/_apis/build/status/CyberBattle-ContinuousIntegration?branchName=master)](https://mscodehub.visualstudio.com/Asterope/_build/latest?definitionId=1359&branchName=master) |
| Docker image | master | [![Build Status](https://mscodehub.visualstudio.com/Asterope/_apis/build/status/CyberBattle-Docker?branchName=master)](https://mscodehub.visualstudio.com/Asterope/_build/latest?definitionId=1454&branchName=master) |




## Benchmark

See [Benchmark](/docs/benchmark.md).


## Setting up a dev environment

It is strongly recommended to work under a Linux environment, either directly or via WSL on Windows.
Running Python on Windows directly should work but is not supported anymore.

Start by checking out the repository:

   ```bash
   git clone https://github.com/microsoft/CyberBattleSim.git
   ```

### On Linux or WSL

The instructions were tested on a Linux Ubuntu distribution (both native and via WSL). Run the following command to set-up your dev environment and install all the required dependencies (apt and pip packages):


```bash
./init.sh
```

The script installs python3.8 if not present. If you are running a version of Ubuntu older than 20 it will automatically add an additional apt repository to install python3.8.

The script will create a [virtual Python environment](https://docs.python.org/3/library/venv.html) under a `venv` subdirectory, you can then
run Python with `venv/bin/python`.

> Note: If you prefer Python from a global installation instead of a virtual environment then you can skip the creation of the virtual envrionment by running the script with `./init.sh -n`. This will instead install all the Python packages on a system-wide installation of Python 3.8.

#### Windows Subsystem for Linux

The supported dev environment on Windows is via WSL.
You first need to install an Ubuntu WSL distribution on your Windows machine,
and then proceed with the Linux instructions (next section).

#### Git authentication from WSL

To authenticate with Git you can either use SSH-based authentication, or
alternatively use the credential-helper trick to automatically generate a
PAT token. The latter can be done by running the following commmand under WSL
([more info here](https://docs.microsoft.com/en-us/windows/wsl/tutorials/wsl-git)):

```ps
git config --global credential.helper "/mnt/c/Program\ Files/Git/mingw64/libexec/git-core/git-credential-manager.exe"
```

#### Docker on WSL

To run your environment within a docker container, we recommend running `docker` via Windows Subsystem on Linux (WSL) using the following instructions:
[Installing Docker on Windows under WSL](https://docs.docker.com/docker-for-windows/wsl-tech-preview/)).


### Windows (unsupported)

This method is not supported anymore, please prefer instead running under
a WSL subsystem Linux environment.
But if you insist you want to start by installing [Python 3.8](https://www.python.org/downloads/windows/) then in a Powershell prompt run the `./init.ps1` script.



## Getting started quickly using Docker (internal only at this stage)

> NOTE: We do not currently redistribute build artifacts or Docker containers externally for this project. We provide the Dockerfile and CI yaml files if you need to recreate those artifacts.

The quickest way to get up and running is to use the Docker container.
Note: you first need to request access to the Docker registry `spinshot.azurecr.io`. (Not publicly available.)

```bash
docker login spinshot.azurecr.io
docker pull spinshot.azurecr.io/cyberbattle:157884
docker run -it spinshot.azurecr.io/cyberbattle:157884 cyberbattle/agents/baseline/run.py
```

## Check your environment

Run the following command to run a simulation with a baseline RL agent:

```
python cyberbattle/agents/baseline/run.py --training_episode_count 1 --eval_episode_count 1 --iteration_count 10 --rewardplot_with 80  --chain_size=20 --ownership_goal 1.0
```

If everything is setup correctly you should get an output that looks like this:

```bash
torch cuda available=True
###### DQL
Learning with: episode_count=1,iteration_count=10,ϵ=0.9,ϵ_min=0.1, ϵ_expdecay=5000,γ=0.015, lr=0.01, replaymemory=10000,
batch=512, target_update=10
  ## Episode: 1/1 'DQL' ϵ=0.9000, γ=0.015, lr=0.01, replaymemory=10000,
batch=512, target_update=10
Episode 1|Iteration 10|reward:  139.0|Elapsed Time: 0:00:00|###################################################################|
###### Random search
Learning with: episode_count=1,iteration_count=10,ϵ=1.0,ϵ_min=0.0,
  ## Episode: 1/1 'Random search' ϵ=1.0000,
Episode 1|Iteration 10|reward:  194.0|Elapsed Time: 0:00:00|###################################################################|
simulation ended
Episode duration -- DQN=Red, Random=Green
   10.00  ┼
Cumulative rewards -- DQN=Red, Random=Green
  194.00  ┼      ╭──╴
  174.60  ┤      │
  155.20  ┤╭─────╯
  135.80  ┤│     ╭──╴
  116.40  ┤│     │
   97.00  ┤│    ╭╯
   77.60  ┤│    │
   58.20  ┤╯ ╭──╯
   38.80  ┤  │
   19.40  ┤  │
    0.00  ┼──╯
```

## Jupyter notebooks

To quickly get familiar with the project you can open one the
the provided Juptyer notebooks to play interactively with
the gym environments. Just start jupyter with `jupyter notebook`, or
`venv/bin/jupyter notebook` if you are using a virtual environment setup.

- Capture The Flag Toy environment notebooks:
  - [Random agent](notebooks/toyctf-random.ipynb)
  - [Interactive session for a human player](notebooks/toyctf-blank.ipynb)
  - [Interactive session - fully solved](notebooks/toyctf-solved.ipynb)

- Chain environment notebooks:
  - [Random agent](notebooks/chainnetwork-random.ipynb)

- Other environments:
  - [Interactive session with a randomly generated environment](notebooks/randomnetwork.ipynb)
  - [Random agent playing on randomly generated networks](notebooks/c2_interactive_interface.ipynb)




## Contributing

This project welcomes contributions and suggestions.  Most contributions require you to agree to a
Contributor License Agreement (CLA) declaring that you have the right to, and actually do, grant us
the rights to use your contribution. For details, visit https://cla.opensource.microsoft.com.

When you submit a pull request, a CLA bot will automatically determine whether you need to provide
a CLA and decorate the PR appropriately (e.g., status check, comment). Simply follow the instructions
provided by the bot. You will only need to do this once across all repos using our CLA.

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/).
For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or
contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.


### Ideas for contributions

Here are some ideas on how to contribute: enhance the simulation (event-based, refined the simulation, …), train an RL algorithm on the existing simulation,
implement benchmark to evaluate and compare novelty of agents, add more network generative modes to train RL-agent on, contribute to the doc, fix bugs.

See also the [wiki for more ideas](https://github.com/microsoft/CyberBattleGym/wiki/Possible-contributions).

## Citing this project

```bibtex
@misc{msft:cyberbattlesim,
  Author = {Brandon Marken and Christian Seifert and Emily Goren and Haoran Wei and James Bono and Joshua Neil and Jugal Parikh and Justin Grana and Kate Farris and Kristian Holsheimer and Michael Betser and Nicole Nichols and William Blum},
  Publisher = {GitHub},
  Howpublished = {\url{https://github.com/microsoft/cyberbattlesim}},
  Title = {CyberBattleSim},
  Year = {2021}
}
```

## Note on privacy
This project does not include any customer data.
The provided models and network topologies are purely fictitious.
Users of the provided code provide all the input to the simulation
and must have the necessary permissions to use any provided data.


## Trademarks

This project may contain trademarks or logos for projects, products, or services. Authorized use of Microsoft
trademarks or logos is subject to and must follow
[Microsoft's Trademark & Brand Guidelines](https://www.microsoft.com/en-us/legal/intellectualproperty/trademarks/usage/general).
Use of Microsoft trademarks or logos in modified versions of this project must not cause confusion or imply Microsoft sponsorship.
Any use of third-party trademarks or logos are subject to those third-party's policies.