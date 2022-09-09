# roscore-novnc
A minimal Docker Composition that runs a ROS (Robot Operating System) _roscore_ process in a container that directs its output to a second, noVNC, container.
This allows GUI programs that are launched in the ROS conmtainer to be viewed in a web browser.

The composition comprises two Docker services:

1. A _ros:noetic-desktop-full_ image, running the _roscore_ process
2. A _theasp/novnc:latest_ image that exposes its web UI on port 8080.

## Use
Open a terminal in the project root directory.

To start the services: 
> docker compose up --build -d

To connect a bash terminal to the ROS service:
> docker compose exec -it roscore bash

Note that the _roscore_ in the last command is the name of the Docker service within the composition.

Then, in the bash shell, to initialise the ROS environment:
> source ros_entrypoint.sh 

Then for example, to run rviz in the bash shell:
> rosrun rviz rviz

Open the UI in a browser at http://localhost:8080/vnc.html

See: 

* https://hub.docker.com/r/theasp/novnc
* https://wiki.ros.org/docker/Tutorials

Inspired by https://github.com/nebohq/mac-ros
