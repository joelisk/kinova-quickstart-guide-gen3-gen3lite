# Quickstart
- Joe Lisk's quick and dirty guide to getting a Kinova Gen3 arm or Gen3 Lite arm up and running (sim + real)
- Read at your own risk and no guarantees anything here will work for your project. This is just a compilation of some basic steps, resources, and troubleshooting common issues I had when running the arms
- Worked on Ubuntu 20.04 + ROS Noetic with RViz & Gazebo for sim and USB for real with the MoveIt library for trajectory planning
- Assuming you already have the arm unboxed and configured. There are online guides for doing so e.g. `kinova gen3 ultra lightweight quick start guide`
- Also [here](https://www.kinovarobotics.com/uploads/User-Guide-Gen3-R07.pdf) is the guide for the Gen3 7dof arm

# AFTER cloning ros_kortex repo [here](https://github.com/Kinovarobotics/ros_kortex) and running the commands to set up the repo:
## Sim:
### To launch:
- run `roslaunch kortex_gazebo spawn_kortex_robot.launch arm:=gen3_lite gripper:=gen3_lite_2f` for Gen3 Lite 6dof
- run `roslaunch kortex_gazebo spawn_kortex_robot.launch arm:=gen3 gripper:=robotiq_2f_85` for Gen3 7dof (+ your specific gripper)
### To run:
- run `rosrun kortex_examples example_move_it_trajectories.py __ns:=my_gen3_lite` or whichever file you've created
- run `rosrun kortex_examples example_move_it_trajectories.py __ns:=my_gen3` or whichever file you've created
## Real:
### To launch:
- run `roslaunch kortex_driver kortex_driver.launch ip_address:=192.168.1.10 arm:=gen3_lite gripper:=gen3_lite_2f` for Gen3 Lite 6dof
- run `roslaunch kortex_driver kortex_driver.launch ip_address:=[your_ip_address] arm:=gen3_lite gripper:=gen3_lite_2f` for Gen3 7dof
### To run:
- run `rosrun kortex_examples example_move_it_trajectories.py __ns:=my_gen3_lite` or whichever file you've created
- run `rosrun kortex_examples example_move_it_trajectories.py __ns:=my_gen3` or whichever file you've created
## A few helpful tips & strategies:
- In simulation, the arm has a MIXED reference frame, with the translation in the {BASE} frame and the rotation in the {TOOL} frame
- When providing a Cartesian pose, the orientation is in quaternions:
  ```
  Actual cartesian pose is : 
    position: 
      x: 0.5799453492566482
      y: 9.385886177843187e-05
      z: 0.43075982195798934
    orientation: 
      x: 0.5055105017532837
      y: 0.498475961346797
      z: 0.4991990536835879
      w: 0.4967707251619618
  ```
- [Here](https://en.wikipedia.org/wiki/Conversion_between_quaternions_and_Euler_angles) is a handy conversion formula w/ C++ code for quaternions and Euler angles
- Ocassionally you may get an Inverse Kinematics error `unable to sample any valid states for goal tree`. One potential workaround is to increase the precision of the orientation values to at least `%.6f`. See [here](https://answers.ros.org/question/322366/moveit-unable-to-sample-any-valid-states-for-goal-tree/) and [here](https://groups.google.com/g/moveit-users/c/yRthi64affg)
- You may also find it useful to add constraints to the motion planning based on your environment and goal. [Here](https://moveit.ros.org/moveit/2020/09/10/ompl-constrained-planning-gsoc.html) is a useful resource for adding path constraints
- You may also find it useful to simulate the Motion Planning in Rviz first and reverse engineer the desired Cartesian pose with a `rospy.loginfo(actual_pose)`. [Here](https://www.youtube.com/watch?v=18mouzWyqRo) is a quick guide for using Rviz in Motion Planning
- The best place to start moving the arms in simulation + real is `/kortex_examples/src/full_arm/example_move_it_trajectories.py`

## When using Kortex Vision or some vision component:
- Coming soon!



