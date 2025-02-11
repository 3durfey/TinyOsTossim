# TinyOS Ubuntu 16 Simulation Setup

This guide provides step-by-step instructions to build and run the TinyOS simulation using Docker.

## Build Docker Image

```sh
docker build -t tinyos-ubuntu16 -f dockerFileUbunto16 .
```

## Run Docker Container

```sh
docker run -it --name tinyos-container [imageHash] /bin/bash
```

## Navigate to the Blink Application

```sh
cd /opt/tinyos-main/apps/Blink
```

## Compile for Micaz Simulation

```sh
make micaz sim
```

## Create `blink-sim.py` Script

Use `nano` to create the simulation script:

```sh
nano blink-sim.py
```

Paste the following content into `blink-sim.py`:

```python
from TOSSIM import *
import sys

# Initialize Tossim
t = Tossim([])
r = t.radio()

# Add debug channels
t.addChannel("BlinkC", sys.stdout)

# Load topology if available
try:
    with open("topology.txt", "r") as f:
        for line in f:
            s = line.split()
            if len(s) == 3:
                r.add(int(s[0]), int(s[1]), float(s[2]))
except IOError:  # Python 2 uses IOError instead of FileNotFoundError
    print("Warning: No topology file found. Running without radio connections.")

# Boot a node
t.getNode(1).bootAtTime(100000)

# Run simulation events
for i in range(1000):
    t.runNextEvent()
```

Save the file and exit (`Ctrl + X`, then `Y`, then `Enter`).

## Run the Simulation

```sh
python2 blink-sim.py
```

---

This will execute the Blink simulation in the TOSSIM environment.
