I am using this repo to control a real HSR robot in docker setup


git clone --recurse-submodules https://github.com/anh0001/hsr_robot_docker.git
docker build -f Dockerfile -t hsr_robot_image .
docker run --net=host --rm -it hsr_robot_image

To modify the setup so that a terminal is open when the container starts, without launching the Gazebo simulator, we need to make changes to both the Dockerfile and the supervisord configuration. Here's how we can do that:

- In the supervisord configuration:
   - We've changed `autostart=true` to `autostart=false` for the simulator program. This means the Gazebo simulator won't start automatically when supervisord runs.
   - We've also changed `autorestart=true` to `autorestart=false` to prevent the simulator from restarting if it stops.

With these changes:

1. When you start the container, you'll be dropped into a bash shell.
2. The Gazebo simulator won't start automatically.
3. You'll have access to all the ROS and Gazebo tools, but they won't be running by default.

If you want to start the Gazebo simulator manually, you can do so from the bash prompt by running:

```bash
roslaunch hsrb_wrs_gazebo_launch wrs_practice0_easy_tmc.launch use_oss_stacks:=true fast_physics:=true highrtf:=true
```

Or, if you want to use supervisord to manage the processes, you can start it manually with:

```bash
/usr/local/bin/supervisord -n -c /etc/supervisor/supervisord.conf
```

And then use `supervisorctl` to start the simulator:

```bash
supervisorctl start simulator
```

These modifications give you more control over when and how you start the Gazebo simulator, while still providing immediate access to a bash terminal when you start the container.

-------------------------------------------------------------------------------
Modified from:
Authors
---------------
 * Yosuke Matsusaka

Contact
---------------
 * HSR Support <xr-hsr-support@mail.toyota.co.jp>

LICENSE
---------------
This software is released under the BSD 3-Clause Clear License, see LICENSE.txt.
