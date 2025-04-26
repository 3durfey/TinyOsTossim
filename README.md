# TinyOS Ubuntu 16 Simulation Setup

This guide provides step-by-step instructions to build and run the TinyOS simulation using Docker. it uses the Docker file named dockerFileUbunto16 included in this repository.

## Build Docker Image

```sh
docker build -t tinyos-ubuntu16 -f dockerFileUbunto16 .
```

## Run Docker Container

```sh
docker run -it --name tinyos-container [imageID] /bin/bash
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

Java Setup

If you would like to use Java in this environment, you will need an older version of the JDK.

Please download JDK 1.5.0_22 from the Oracle Java Archive:

🔗 Oracle Java SE 5 Archive Download

Download the following file:
	•	jdk-1_5_0_22-linux-amd64.bin

Then:
	1.	Move the downloaded .bin file into your Docker container or project directory.
	2.	Inside the container, unzip and execute the file to install the JDK.
