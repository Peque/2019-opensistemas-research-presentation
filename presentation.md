% R&D at Opensistemas
% Miguel Sánchez de León Peque
% 2019-11-08


# Introduction

## Hi! :wave:

- [Miguel Sánchez de León Peque](https://www.linkedin.com/in/peque)
- [OpenSistemas](http://www.opensistemas.com)
- Data scientist
- Programming, data, machine learning
- Python :heart:

## Abstract

- TODO
- ...


# Introduction

## Market data

## Candles

## Trading

## Beasts

- Spreads
- Dynamics
- Noise


# Portfolio optimization

## Why?

- Increase diversification
- Reduce correlation

## Architecture

**Generator**
: Generates random configurations in the parameters subspace

**Backtester**
: Simulates the configuration and filters

**Portfolio**
: Includes the configuration if the portfolio performance improves

## Basic architecture

![](static/simple-architecture.svg)

## Flexible

![](static/complex-architecture.svg)

## Genetic algorithms

- Explore further on good results
- High correlation makes the environment rules change
- More hyperparameters/complexity

## Exploration

![](static/backtest-exploration-subspace.png)

*Note variables' importance!*

## 9 grid

![](static/backtest-exploration-grid.png)

## 9 random

![](static/backtest-exploration-random.png)

## 6 random

![](static/backtest-exploration-random-few.png)

## 64 grid

![](static/backtest-exploration-grid-many.png)

## 32 random

![](static/backtest-exploration-random-many.png)

## Smoothing

![](static/smoothing.png){width=60%}

## Filtering

- Daily activity
- Sharpe
- Sortino

$$
S_a = \frac{E[R_a - R_b]}{\sigma_a}
$$

## Evaluation (optimization)

- Performance
- Pearson correlation

![](static/correlation-matrix.png)

## Advantages

- Faster convergence
- Traceable overfit
- Flexible ranges and depth


# Walk-forward simulation


# Deep learning


# Motivation

## Search engine results

- TradeStation
- Metatrader
- NinjaTrader
- Many others...

## Cons

- Proprietary
- Heavily desktop oriented
- Linux support?
- Can I use Python to create my strategies?

---

![](./static/cry.gif){width=60%}

## What we wanted

- Control
- Optional GUI
- Multiplatform
- Python

----

> Is this possible?

---

![](./static/dramatic-pause.gif){width=80%}

# osMarkets

## Basics

- **Broker-independent** platform
- Implemented **over osBrain**
- Designed for **real-time automated trading**

## Overview

![](./static/architecture.svg){width=100%}

## Feeder

- Get market data
- Multithread

## Overview

![](./static/architecture.svg){width=100%}

## Router

- Broker independent provider
- Stores/distributes
- Update/resample

## Market data

- Tick
- Bar
- Bar Series

## Bar series

![](./static/barseries.svg){width=100%}

## Overview

![](./static/architecture.svg){width=100%}

## Brain

- Subscriptions
- Run algorithms
- Send orders
- Great ecosystem
- Can do anything

## Abstraction

![](./static/brain-hierarchy-grayscale.svg){width=60%}

## Overview

![](./static/architecture.svg){width=100%}

## Trader

- Handle orders
- Multithread

## Example

![](./static/architecture_2.svg){width=100%}

## Other agents

- Logger
- Informer
- Overmind

# GUI

## Features

- Real-time **market data visualization**
- Real-time **indicators visualization**
- **Integration** with osMarkets

## Qt

- Great **portability**
- Probably the **most widely used**
- LGPL

## PyQtGraph

- **Pure Python** graphics library
- **Fast** real-time display
- User **interaction**
- MIT

## Overview

- [Main Window](./video/gui_main_window.webm)
- [Chart](./video/gui_chart.webm)
- [Indicators](./video/gui_indicators.webm)

## Note

![](./static/architecture_gui.svg){width=80%}

# osBrain

## Basics

> A **general purpose** multi-agent system

- Independent agents
- Message passing
- Easy configuration and deployment

## Concurrency

- Locks
- Semaphores
- Critical sections

Code **fails**. Misterious bugs...

---

Irreversible disorders...

![](./static/developers.gif){width=70%}

## Relativity

$$
e = mc^2
$$

. . .

- $e \equiv$ energy (effort)
- $m \equiv$ mass (lines of code)
- $c \equiv$ ~~speed of light~~ concurrency

## Multi-agent systems

- Autonomy of the agents
- Local views
- Decentralization

## Other use cases

![](./static/portfolio_osbrain.svg){width=100%}

## Processes vs. threads

Have you met GIL?

## ØMQ

> ØMQ is very cool -
> If you don't have a project that needs it... ¡create one!

## Why?

- Higher level than raw sockets
- Asynchronous communication
- Multiconnetion (not necessarily one-to-one).
- Multipattern (REQ-REP, PUB-SUB...).
- Multitransport (inproc, ipc, tcp, pgm, epgm).
- Multilanguage (C, C++, Python, Scala, Haskell, Go...).
- Multiplatform.
- Scalable (threads, processes, machines)
- LGPL.

----

![](./static/not_bad.jpg)

## A basic agent

- Is a system **process**
- Implements methods for easy **binding** and **connecting** with different
  **patterns**
- Activates on **incoming message**
- Mulithreading may be used (*inproc* socket)

## Configuration

> Agents are independent, but they must know the addresses of other agents


- There may be **many sockets**!
- For most agents, assigning a **random address** is simpler

## If only we could...

- Have a **name server**
- Have an **Overmind**...

## Pyro4

- **PY**thon **R**emote **O**bjects
- Treat remote objects as local
- Already has a name server implementation

## A real agent

- Is a system **process**
- This process runs a **Pyro multiplexed server**
- The server serves an actual **Agent object**
- Main thread runs the **main loop** (one-way)
<!--
- Other threads allow for **concurrent access for configuration** (*c = 2*)

## Safe calls

```python
def example(self, x):
    self.x = x

def example_safe(self, x):
    self.call_safe('example', x)
```
-->

## FOSS! :tada:

![](./static/osbrain-logo-name.svg){width=15cm}

- [https://pypi.python.org/pypi/osbrain](https://pypi.python.org/pypi/osbrain)
- [https://github.com/opensistemas-hub/osbrain](https://github.com/opensistemas-hub/osbrain)
- [https://osbrain.readthedocs.io/](https://osbrain.readthedocs.io/)

## Conclusion

- General-purpose multi-agent system with Python
- Independent agents (processes)
- Message passing using ØMQ
- Easy deployment and remote configuration using Pyro4

# Code examples

## An example (I)

```python
import time
from osbrain import random_nameserver
from osbrain import run_agent


def log_message(agent, message):
    agent.log_info('received: %s' % message)


if __name__ == '__main__':

    # System deployment
    ns = random_nameserver()
    sender = run_agent('Alice', ns)
    receiver = run_agent('Bob', ns)

    # System configuration
    addr = sender.bind('PUSH', alias='main')
    receiver.connect(addr, handler=log_message)

    # Send messages
    while True:
        time.sleep(1)
        sender.send('main', 'Hello, world!')
```


## An example (II)

```python
from osmarkets.brain import Brain
from osmarkets.architecture import OandaArchitecture


class Example(Brain):
    def on_new_bar(self, series):
        # Algorithm
        if series[0].close > series[1].close:
            side = 'buy'
        else:
            side = 'sell'
        # Order
        self.send_order(side=side,
                        size=100,
                        symbol=series.bsid.symbol)


if __name__ == '__main__':

    system = OandaArchitecture(logging=True,
                               account_id=123456,
                               access_token='123abc-345def',
                               environment='live')
    system.stream(['EUR_USD'])

    system.add_brain('example', base=Example)
    system['example'].set_attr(DEBUG=True)
    system['example'].subscribe(('EUR_USD', 1, 'Minutes'), 100)
```


# Development methodology

## There are 3 development methodologies

. . .

![](static/hahaha.gif){width=80%}

## Ours

- TDD
- Integration in `master`
- All code is reviewed
- CI
- Forks

## ¿Is it *the* good one? :joy:

- Muy utilizada
- La más común en proyectos libres
- Aplicable a cualquier proyecto

## TDD :white_check_mark:

. . .

#. Write the test
#. Write the code
#. "Clean up"

## Integration in `master`

Flows:

- ~~Git flow~~
- GitHub
- Gitlab

---

![](static/gitflow.png){width=50%}

## GitHub

![](static/github_flow.png)

## Gitlab

![](static/environment_branches.png){width=50%}

---

![](static/release_branches.png){width=50%}

## Reviews :+1:

- Pull/merge requests
- Discussion
- Comments on the code

## Continuous integration

- Small changes
- Infrastructure

## Fork

- Group/organization vs. user
- `upstream` vs. `origin`
- Clean
- Secure
- Restricted permissions

## Sharing your fork

![](static/fork.svg)

## Version control systems

![](static/git_services.png)


# CI/CD :rocket:

## Virtual environments

- `venv`
- Conda
- Pipenv
- Poetry

## Testing :mag:

- `pytest --pdb` (`pdbpp`)
- Tox
- Gitlab CI
- Gitlab registry

## Lint and style :sunglasses:

- [Black](https://github.com/psf/black)
- [`flake8`](https://github.com/pycqa/flake8)
- `flake8` plugins ([`isort`](https://github.com/timothycrosley/isort), ...)
- [Mypy](http://www.mypy-lang.org/)

## Other

- Coverage - [codecov.io](https://codecov.io)
- Cleanness - [codeclimate.com](https://codeclimate.com)
- Security - [lgtm.com](https://lgtm.com/)

# Documentation :book:

## [Sphinx](https://www.sphinx-doc.org)

![](static/sphinx-logo.png)

## Syntax

```rst
Title
=====

Paragraph with `a link <https://domain.com>`_.

- Unordered
- List

.. code-block:: python

   print("Hello world!")
```

## Links :link:

```rst
Pargraph with `a link`_.

.. _a link: https://domain.com
```

```rst
.. _label:

One section
-----------

This is the text of the section.


Another section
---------------

Refer to :ref:`label`.
```

## Other directives :heart_eyes:

```rst
.. warning:: This is a warning!
```

```rst
.. literalinclude:: path/to/code.py
   :lines: 1,3,5-10
   :emphasize-lines: 12,15-18
```

```rst
.. figure:: myfigure.svg
   :scale: 50 %
   :align: left

   Figure caption.
```

## More

[Matplotlib :heart: Sphinx](https://matplotlib.org/sampledoc/extensions.html)

```rst
.. plot::

   import matplotlib.pyplot as plt
   import numpy as np

   x = np.random.randn(1000)
   plt.hist(x, 20)
   plt.title("Normal distribution")
   plt.show()
```

## Even more! :heart_eyes: :heart_eyes: :heart_eyes:

```rst
Mounting
--------

The mounting has been designed with `CadQuery`_. See the
`Bulebule 3D designs`_.

.. cadquery:: ../../3d/mount.py
   :select: mount
```

- [Visualization](https://bulebule.readthedocs.io/en/latest/building.html#mounting)

## Deployment

- Gitlab Pages
- Read the Docs


# That's all! :tada: :beers: :smile:

## Thanks! :heart:
