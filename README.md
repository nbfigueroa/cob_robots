# cob_robots
## Version
* This is based on the release version ros-indigo-cob-robots:amd64/trusty **0.6.7**-0trusty-20170809-120350-0800, and incorporated with the changes customized for Robbie & Yuri (cob raw3-6) in Interactive Robotics Group.
  * To mix a release version with customizations:
    * `git clone https://github.com/ipa320/cob_robots.git`
    * `git checkout 0.6.7`
    * `git remote -v`
    * `git remote rename origin upstream`
    * `git remote add origin <your forked git repo url>`
      * `git remote add origin https://github.com/Shentheman/cob_robots.git`
    * `git fetch origin`
    * `git checkout -b <your new branch name>`
      * `git checkout -b irg-raw3-6`
    * `git status`
    * `git add -A`
    * `git commit -am <commit messages>`
    * `git push origin master`

## MoveIt! Configuration
* MoveIt! configuration is located at `./src/cob_robots/cob_moveit_config`. The *SRDF* file is located at `/home/shen/mit/1111/src/cob_robots/cob_moveit_config/robots/raw3-6/moveit/config`.
  * If you add new objects to the robot urdf, then you can use [**MoveIt! Setup Assistant**](http://docs.ros.org/hydro/api/moveit_setup_assistant/html/doc/tutorial.html) to update the SRDF file. It is really hard to update the self-collisions to disable inside the SRDF file manually since there are a lot of them, so it is better to first use the setup-assistant to auto-generate the config files.
    * `$ roslaunch moveit_setup_assistant setup_assistant.launch`
    * The setup-assistant also generates launch files for MoveIt!, but we don't need to update our original launch files if not necessary.
    * [Only using the setup-assistant is not enough to use Moveit, you need to provide Moveit a way to control your robot, through a controller manager.](https://answers.ros.org/question/167501/how-to-solve-parameter-moveit_controller_manager-not-specified/)
      * We already have the `controller.yaml` at `./src/cob_robots/cob_moveit_config/robots/raw3-6/moveit/config/controllers.yaml`, so we can just copy the original `controller.yaml` when we update the SRDF.
  * After using setup-assistant, you will have the configuration files including the SRDF file. However, there might still be some self-collisions which are not disabled by the setup-assistant. So we have to add them by ourselves manually. We can use this Python script at `./src/python_utils/scripts/generate_self_collisions_for_SRDF.py` to auto-generate the SRDF code to disable self-collision based on lists of robot URDF links.
