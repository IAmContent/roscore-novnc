# roscore-novnc
A minimal Docker Composition that runs a ROS (Robot Operating System) _roscore_ process in a container, directing its DISPLAY to a noVNC server, running in a second container.

This allows GUI programs that are launched in the ROS container to be viewed in a web browser.

The composition comprises two Docker services:

1. A _ros:noetic-desktop-full_ image, running the _roscore_ process
2. A _theasp/novnc:latest_ image that exposes its web UI on port 8080.

## Using Docker Compose 
### Running it
Open a terminal in the project root directory.

To start the services:
```
docker compose up --build -d
```

### Using it

To connect a bash terminal to the ROS service:
```
docker compose exec -it roscore bash
```

Note that _roscore_ in the last command is the name of the Docker service within the composition.

A more Docker-like approach would be to run bash in a new container on the same Docker network and direct its DISPLAY to the noVNC server:
```
docker run -it --net=roscore-novnc_ros --env="DISPLAY=novnc:0.0" --env="ROS_MASTER_URI=http://roscore:11311" osrf/ros:noetic-desktop-full bash
```

Note that _roscore-novnc_ros_ in the last command is the compound name of the _ros_ Docker network within the _roscore-novnc_ composition. Also note that _roscore_ in the last command is the name of the Docker service within the composition.

Then, in the bash shell, to initialise the ROS environment:
```
source ros_entrypoint.sh 
```

Then, for example, to run rviz in the bash shell:
```
rosrun rviz rviz
```

Open the UI in a browser at http://localhost:8080/vnc.html , changing _localhost_ to your server name or IP address if you have deployed to a different host.


## Alternatively, using plain Docker (without compose)
### Running it
To run the same containers manually (witout using Docker Compose), first create the Docker network by running the folowing in a terminal:
```
docker network create ros
```

Then start the noVNC container:
```
docker run -d --net=ros --env="DISPLAY_WIDTH=3000" --env="DISPLAY_HEIGHT=1800" --env="RUN_XTERM=no" -p 8080:8080 theasp/novnc:latest
```

Then start the ROS desktop container, running _roscore_:
```
docker run -d --net=ros --env="DISPLAY=novnc:0.0" --name roscore osrf/ros:noetic-desktop-full roscore
```

## Using it

To connect a bash terminal to the ROS service:
```
docker exec -it roscore bash
```
Note that _roscore_ in the last command is the name of the Docker service within the composition.

A more Docker-like approach would be to run bash in a new container on the same Docker network and direct its DISPLAY to the noVNC server:
```
docker run -it --net=ros --env="DISPLAY=novnc:0.0" --env="ROS_MASTER_URI=http://roscore:11311" osrf/ros:noetic-desktop-full bash
```

Note that _roscore_ in the last command is the name of the Docker service within the composition.

Then, in the bash shell, to initialise the ROS environment:
```
source ros_entrypoint.sh 
```

Then, for example, to run rviz in the bash shell:
```
rosrun rviz rviz
```

Open the UI in a browser at http://localhost:8080/vnc.html , changing _localhost_ to your server name or IP address if you have deployed to a different host.


## References
See: 

* https://hub.docker.com/r/theasp/novnc
* https://wiki.ros.org/docker/Tutorials

Inspired by https://github.com/nebohq/mac-ros

## License

This software (excluding the software on which it depends) is published under the [MIT License](https://opensource.org/licenses/MIT).
