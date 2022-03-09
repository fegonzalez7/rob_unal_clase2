# Robotics - UNAL - LAB2
## Requirements
Modern robotics relies on the versatile middleware known as ROS (Robotic Operative System). Some of the most useful tools in robotics run in this framework. Learning ROS could lead to insanity (jikes...) but fortunately with time and patience (tons of), all of you young padawans will master the power of ROS.

### Linux
**ROS runs on linux, period.**

There are a couple of ways to run Linux:
- Native installation :sunglasses: (Recommended, almost mandatory)
- Virtual machine :baby: (Almost everything works, but intensive GPU apps will crash)
- Docker :new: (Too new)
- Linux substrate for windows :alien: (Fancy, but it has some compatibility issues, and it has no GUI)

For this course, the Linux of choice is [Ubuntu 20.04 LTS](https://ubuntu.com/download/desktop "Ubuntu 20.04 LTS"). In this [link](https://www.tecmint.com/install-ubuntu-20-04-desktop/ "link") you will find a good installation tutorial.

<img src="https://images.wondershare.com/recoverit/article/11/linux-vs-windows-8.jpg" alt="drawing" width="600"/>

### ROS

<img src="https://www.ros.org/imgs/logo-white.png" alt="drawing" width="300"/>

Once Linux has been installed, it's ROS time. For this course, ROS Noetic will be used. The installation steps for ubuntu are [here](http://wiki.ros.org/noetic/Installation/Ubuntu "here"), for other Linux distros look [here](http://wiki.ros.org/noetic/Installation "here").

In any case, I left the instructions in this repo as well. Just open a new terminal and run the following commands:

```console
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt install curl
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
sudo apt update
sudo apt install ros-noetic-desktop-full
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
sudo apt install python3-rosdep python3-rosinstall python3-rosinstall-generator python3-wstool build-essential
sudo apt install python3-rosdep
sudo rosdep init
rosdep update
```
Please visit the [Frobs_RL](https://github.com/jmfajardod/frobs_rl "Frobs_RL") repo, leave a star and salute FajardROS (One of the only few guys that has master ROS).

### Recommended apps/packages
After years of trying learning ROS, I have compiled some useful tools, some of them have been suggestions, some have been my own discovery.

#### VSCode
Thanks to Microsoft for one of the best code editors in the world. I strongly recommend it. [Here](https://code.visualstudio.com/docs/setup/linux "Here") there is the official way to install VScode on Ubuntu.

#### Kitty
Linux terminal is really powerful, when you master the shell you are gonna look kinda hackerish. Nonetheless, the default terminal in Ubuntu could be constrained. FajardROS found Kitty, since then I started loving cats. [Link](https://sw.kovidgoyal.net/kitty/binary/ "Link") to official page, and link for the installation [guide](https://connectwww.com/how-to-install-kitty-on-ubuntu-kitty-terminal-emulator/61186/ "guide").

#### Catkin build
Once you install ROS you realize that catkin is a package compiler (kinda). *catkin_make* compiles the entire workspace, there is no harm if you have a couple of light packages, but if you have a lot of packages in one workspace, I recommend you to install *catkin_build*. It will allow compiling the packages separately. Open a terminal (hopefully kitty) and run:

```console
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -sc` main" > /etc/apt/sources.list.d/ros-latest.list'
wget http://packages.ros.org/ros.key -O - | sudo apt-key add -
sudo apt-get update
sudo apt-get install python3-catkin-tools
```

### Git
If you are reading this, you will probably have some experience with repositories,  in the case you do not have it in your machine, just open a terminal and run:

```console
sudo apt update
sudo apt install git
git --version
```
## Python and C++
Fortunately, python3 interpreter and C compiler are preinstalled on Linux, so no worries it is not Windows.

------------

If I found more tools, I will add them here. If you find something interesting don't hesitate of letting me know.

------------

## Turtlesim

Now the real businesses.

Clone the *hello_turtle* repo from [here](https://github.com/felipeg17/hello_turtle). It's a sort of hello world in the ROS community. Some changes have been added, but don't worry, during the class you will learn most of the concepts.

##### Run ROSCORE and Turtlesim_node
First, open 3 terminals (if you use Kitty press ctrl + shift + enter three times) and run:
**First terminal**
```console
roscore
```
**Second terminal**
```console
rosrun turtlesim turtlesim_node
```
![alt text](https://i.postimg.cc/j2TbKyPc/Screenshot-from-2022-03-04-12-10-27.png)

You will get something like this:
![alt text](https://i.postimg.cc/FsD45dRb/Screenshot-from-2022-03-04-12-10-50.png)
If you are lucky enough, you will get a better turtle icon than me.

##### Nodes and Topics
In the third terminal run:
**Third terminal**
```console
rostopic list
rosnode list
```
![alt text](https://i.postimg.cc/bwrjzyD7/Screenshot-from-2022-03-04-12-11-17.png)

You will see all topics and nodes running. Notice that the topics related to the turtle simulation have the */turtle1* namespace.

Now in the same terminal run:
**Third terminal**
```console
rosrun turtlesim turtle_teleop_key
```
It will allow you to move the turtle around using the keys.

Open a new terminal and run:
**Fourth terminal**
```console
rqt_graph
```
![alt text](https://i.postimg.cc/0NJRwVCZ/Screenshot-from-2022-03-04-12-13-44.png)
A graphical visualization of the topics and nodes will appear, It shows the interconnections between them.

------------
## hello_turtle package

The hello_turtle package somehow summarizes the previous concepts. It introduces the ROS-package basics.

Firstly look at the files and dirs structure:
![alt text](https://i.postimg.cc/tTNR0k7q/Screenshot-from-2022-03-04-13-47-28.png)

It resembles the ROS recommended structure.
![alt text](https://miro.medium.com/max/1400/0*vfNM1mbkhUpvK-nW.png)

The package consists of: 
- **CMakelist.txt:** A set of special instructions to the compiler (more details in class).
- **package.xml:** An xml file to specify the package dependencies.
- **/config:** A directory to store all configuration files. There is a new standard in ROS called *.yaml* (we will call it *yamile*) which is basically an *xml* with more functionalities.
- **/launch:** This folder stores all the launch files (more details above).
- **/scripts:** This dir stores all the package executables (mainly python one). 
- **/src:** Where all C stuff is stored.

There are a couple of missing folders, for that I **strongly** suggest (*command*) to follow these ROS tutorials:
 - [**ROS packages**:](http://wiki.ros.org/ROS/Tutorials/CreatingPackage) Shows how a package is created.
 - [**ROS packages 2**:](http://wiki.ros.org/ROS/Tutorials/BuildingPackages): Shows how to actually build the package.
 - [**ROS messages and services**:](http://wiki.ros.org/ROS/Tutorials/CreatingMsgAndSrv) Shows how to create new ROS msgs and ROS srv, there you will learn the purpose of the *msg* and *srv* dirs.
 - [**ROS subs and pubs**:](http://wiki.ros.org/ROS/Tutorials/WritingPublisherSubscriber%28python%29) Shows how to write a subscriber and publisher in Python.
 - [**ROS service running**:](http://wiki.ros.org/ROS/Tutorials/WritingServiceClient%28python%29): Shows how to write a pair client and service in Python.

Before continuing, close all terminal and ROS processes. In a new clean terminal run:
```console
cd ~/catkin_ws/src
git clone https://github.com/felipeg17/hello_turtle.git
```
It cloned the *hello_turtle* package in the *catkin_ws*.

```console
cd ~/catkin_ws/
catkin build hello_turtle 
```
If you installed *catkin tools*

Or
```console
cd ~/catkin_ws/
catkin make
```
If you want to compile all packages

You will get something like this: (Ignore the warnings, it's just cmake crying)
![atl text](https://i.postimg.cc/Xqm40zKC/Screenshot-from-2022-03-04-14-22-27.png)

Now we source the changes:
```console
cd ~/catkin_ws/
source devel/setup.bash
```

Moment of true, run this command:
```console
roslaunch hello_turtle turtle.launch
```
![alt text](https://i.postimg.cc/hPXVbQfy/Screenshot-from-2022-03-04-14-29-48.png)
But what just happened?....it run the roscore, the turtlesim, and all that stuff with just one command, that's the power of launch files.

### The launch file
It condense multiple commands in just one executable file. Let's take a look:
```xml
<launch>
  <node
    pkg="turtlesim" 
    name="sim" 
    type="turtlesim_node">
    <rosparam file="$(find hello_turtle)/config/params.yaml" command="load" ns=""/>
  </node>
</launch>
```
It creates a node from the *turtlesim* package with the name *sim*, additionally, it loads some parameters stored in the *config/params.yaml* (spoiler: those params simply change the canvas color). The latter is really important because a lot of packages load info from different configuration files, so remember this syntax. At the same time the launch file *launches* *roscore* if it is needed.

The other parts of the packages will be treated in class, so try to attempt.

------------

**Acknowledgments:**

 - Jose Manuel Fajardo
 - Sebastian Realpe 
 - Hans Milos Toquica

This repo has taken a lot of effort, so consider to leave a star, [follow me](https://felipeg17.github.io/index.html), and if you feel generous I have [Paypal](https://paypal.me/fegonzalez17?country.x=CO&locale.x=en_US) (just kidding).
