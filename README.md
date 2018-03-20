[![Build Status](https://travis-ci.org/AdamStelmaszczyk/dqn.svg?branch=master)](https://travis-ci.org/AdamStelmaszczyk/dqn)

TensorFlow + Keras implementation of deep Q-learning.

## Hardware

If TensorFlow finds a GPU you will see `Creating TensorFlow device (/device:GPU:0)` in the beginning of log
and the code will use 1 GPU + 1 CPU. If it doesn't find a GPU, it will use 1 CPU.

Tesla K40 + Intel i5 Haswell give about 80 steps/s during training.

Training a single game requires 1M training + 200k evaluation steps (20k evaluation steps every 100k training steps).
1.2M steps takes about 3.5 hours with K40.

I'd recommend about 10 GB of RAM to safely train.
REPLAY_BUFFER_SIZE = 100000 and stacking 4 frames in the observation already uses 84 \* 84 \* 4 \* 100000 = 2.6 GB RAM.


## Install

1. Clone this repo: `git clone https://github.com/AdamStelmaszczyk/dqn.git`.
2. [Install `conda`](https://conda.io/docs/user-guide/install/index.html) for dependency management.
3. Create `dqn` conda environment: `conda create -n dqn python=3.5.2 -y`.
4. Activate `dqn` conda environment: `source activate dqn`. All the following commands should be run in the activated `dqn` environment.
5. Install basic dependencies: `pip install -r requirements.txt`.
6. If you wish to use a GPU: `pip install -r requirements-gpu.txt`.

There is an automatic build on Travis which [does the same](https://github.com/AdamStelmaszczyk/dqn/blob/master/.travis.yml).

## Uninstall

1. Deactivate conda environment: `source deactivate`.
2. Remove `dqn` conda environment: `conda env remove -n dqn -y`.

## Usage

Basic file is `run.py`.

```
usage: run.py [-h] [--env ENV] [--images] [--model MODEL] [--name NAME]
              [--play] [--seed SEED] [--test] [--view]

optional arguments:
  -h, --help     show this help message and exit
  --env ENV      Atari game name (default: Breakout)
  --images       save images during evaluation (default: False)
  --model MODEL  model filename to load (default: None)
  --name NAME    name for saved files (default: 03-20-19-55)
  --play         play with WSAD + Space (default: False)
  --seed SEED    pseudo random number generator seed (default: None)
  --test         run tests (default: False)
  --view         view the evaluation in a window (default: False)
```

### Train

`python run.py --env Pong`

There are 60 games you can choose from:

`AirRaid, Alien, Amidar, Assault, Asterix, Asteroids, Atlantis, BankHeist, BattleZone, BeamRider, Berzerk, Bowling, Boxing, Breakout, Carnival, Centipede, ChopperCommand, CrazyClimber, DemonAttack, DoubleDunk, ElevatorAction, Enduro, FishingDerby, Freeway, Frostbite, Gopher, Gravitar, Hero, IceHockey, Jamesbond, JourneyEscape, Kangaroo, Krull, KungFuMaster, MontezumaRevenge, MsPacman, NameThisGame, Phoenix, Pitfall, Pong, Pooyan, PrivateEye, Qbert, Riverraid, RoadRunner, Robotank, Seaquest, Skiing, Solaris, SpaceInvaders, StarGunner, Tennis, TimePilot, Tutankham, UpNDown, Venture, VideoPinball, WizardOfWor, YarsRevenge, Zaxxon`

### Play using the same observations as DQN

`python run.py --play`

Keys:

- <kbd>W</kbd> - up
- <kbd>S</kbd> - down
- <kbd>A</kbd> - left
- <kbd>D</kbd> - right
- <kbd>SPACE</kbd> - fire button (concrete action depends on a game)

### Generate GIFs

1. Generate images: `python run.py --images --model=YOUR_MODEL.h5`.
2. We will use `convert` tool, which is part of ImageMagick, [here](https://www.imagemagick.org/script/download.php) are the installation instructions.
3. Convert images from episode 1 to GIF: `convert -layers optimize-frame 1_*.png 1.gif`


## Links

- https://www.cs.toronto.edu/~vmnih/docs/dqn.pdf
- https://web.stanford.edu/class/psych209/Readings/MnihEtAlHassibis15NatureControlDeepRL.pdf
- https://becominghuman.ai/lets-build-an-atari-ai-part-1-dqn-df57e8ff3b26
- https://blog.openai.com/openai-baselines-dqn
- https://danieltakeshi.github.io/2016/11/25/frame-skipping-and-preprocessing-for-deep-q-networks-on-atari-2600-games/
- https://github.com/openai/baselines/blob/master/baselines/common/atari_wrappers.py
- https://github.com/openai/gym/blob/master/gym/envs/__init__.py#L483
- https://github.com/kuz/DeepMind-Atari-Deep-Q-Learner
- https://medium.com/mlreview/speeding-up-dqn-on-pytorch-solving-pong-in-30-minutes-81a1bd2dff55
