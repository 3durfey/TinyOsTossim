TinyOS Docker Setup and Simulation

Building the Docker Image

Build the TinyOS Docker image:

docker build -t tinyos-ubuntu16 -f dockerFileUbunto16 .

Running the Docker Container

Run the container with interactive mode:

docker run -it --name tinyos-container 64859f61a7e4 /bin/bash

Compiling and Running the Blink Simulation

Navigate to the Blink application directory:

cd /opt/tinyos-main/apps/Blink

Compile the simulation:

make micaz sim

Creating the Simulation Script

Open blink-sim.py using Nano:

nano blink-sim.py

Add the following script:

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

Save and exit Nano (CTRL + X, then Y, then Enter).

Running the Simulation

Execute the simulation script:

python2 blink-sim.py

This will run the Blink simulation inside the TinyOS Docker container. 🚀

