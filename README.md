# <p align="center">Automated Planning: Theory and Practice - Project</p> 

<p align="justify">This repository represents a student project by Carlo Marotta for the "Automated Planning" course of the Master's in Artificial Intelligence Systems at the University of Trento, for the academic year 2022-2023.</p>

The repository consists of the following components:

- Assignment PDF: This document provides the project's assignment details.
- Problem Folders: There are 5 separate folders for each of the five problems described in the assignment.
- Report PDF: This document contains the project report.
- README file: it serves as a guide to install all the necessary materials required to run the code, how to execute it and the obtained results in form of screenshot.

Here's a brief description of the five different tasks:

* <p align="justify">Problem 1: it involves developing the basic structure of the problem using the Planning Domain Definition Language (PDDL). The problem revolves around a robotic agent responsible for delivering necessary contents to injured people. To run this problem, you will need the Planutils environment and the FastDownward planner.</p>

* <p align="justify">Problem 2: it is an extension of the previous problem, introducing a carrier that enables the robotic agent to transport plus contents more efficiently. It also utilizes PDDL.</p>

* <p align="justify">Problem 3: the PDDL problem from Problem 2 is converted to the Hierarchical Task Network Planning Domain Definition Language (HDDL). Tasks and methods are introduced and made compatible with the existing actions. To run this task, you will need the Panda planner.</p>

* <p align="justify">Problem 4: it extends Problem 2 by introducing a temporal domain. Each action is assigned a specific duration and time constraints are incorporated. The goal is to minimize the required time. To run this task, you can use either the Optic planner or the Temporal Fast Downward planner.</p>

* <p align="justify">Problem 5: it further extends Problem 4 by employing a more sophisticated planner that allows the definition of C++ code for each action. Once a plan is obtained, which minimizes the required time, it is possible to simulate the proposed solution by running the plan using the implemented C++ codes. To run this problem, you will need ROS2 and PlanSys2.</p>

The mentioned planners are available and ready to install on Linux. It is recommended to use a Linux machine for the best experience. Alternatively, you can install [Ubuntu 22.04](https://ubuntu.com/download/desktop) for the [VirtualBox](https://www.virtualbox.org) directly on MacOS. Instructions for installing it are provided in the subsequent section. If you are using Linux, you can proceed directly to the section that explains the installation process for the planners on Linux.

****
# Install all the planners in Ubuntu 22.04 using VirtualBox for MacOS
<p align="justify">At the beginning, to update your packages and prepare for the next steps, please run the following commands:</p>

```bash
sudo apt update
sudo apt upgrade
sudo apt install -y python3-pip
```

## GO and SINGULARITY
<p align="justify">We require Singularity, a container platform, for installation. After installing Singularity, you can install Planutils using pip3.</p>

1. Install GO
```bash
sudo apt update
sudo apt upgrade
sudo apt-get install -y curl vim git build-essential libseccomp-dev pkg-config squashfs-tools cryptsetup
export VERSION=1.20.3 OS=linux ARCH=amd64  # change this with the more recent in the following GO website (https://go.dev/dl/)
sudo rm -r /usr/local/go
wget -O /tmp/go${VERSION}.${OS}-${ARCH}.tar.gz https://dl.google.com/go/go${VERSION}.${OS}-${ARCH}.tar.gz && \
sudo tar -C /usr/local -xzf /tmp/go${VERSION}.${OS}-${ARCH}.tar.gz
echo 'export GOPATH=${HOME}/go' >> ~/.bashrc
echo 'export PATH=/usr/local/go/bin:${PATH}:${GOPATH}/bin' >> ~/.bashrc
source ~/.bashrc
curl -sfL https://install.goreleaser.com/github.com/golangci/golangci-lint.sh |
sh -s -- -b $(go env GOPATH)/bin v1.21.0
mkdir -p ${GOPATH}/src/github.com/sylabs
cd ${GOPATH}/src/github.com/sylabs
```

2. Install SINGULARITY
```bash
git clone https://github.com/sylabs/singularity.git
cd singularity
git checkout v3.6.3
cd ${GOPATH}/src/github.com/sylabs/singularity
./mconfig
cd ./builddir
make
sudo make install
```
  
3. Change Singularity mount hostfs option
```bash
cd /usr/local/etc/singularity
sudo chmod a+rwx singularity.conf
vim singularity.conf

# Press the 'shift+I' keys to enter insert mode.
# Scroll down the file until you locate the line containing the phrase "mount hostfs = no".
# Modify the line by replacing "mount hostfs = no" with "mount hostfs = yes".
# Press the 'esc' key to exit insert mode.
# Type ':x' and press enter to save the changes and exit the file.
```

## Apptainer and PLANUTILS
1. Install APPTAINER
<p align="justify">To enable Fast Downward support in your system, you need to install Apptainer beforehand. Installing Apptainer facilitates the containerization of Fast Downward, enabling you to create self-contained and portable environments for running Fast Downward effectively.</p>

```bash
cd
sudo apt update
sudo apt upgrade
sudo apt install -y software-properties-common
sudo add-apt-repository -y ppa:apptainer/ppa
sudo apt update
sudo apt install -y apptainer
```

2. Install PLANUTILS
<p align="justify">Planutils is a suite of planners that simplifies the installation and usage of planners within a virtual environment. Once installed, you can conveniently use the suite of planners within the provided virtual environment.</p>

```bash
sudo pip install planutils
planutils activate
```

3. Setup Planutils.
```bash
planutils setup
exit
```

4. Install DOWNWARD, FF, LAMA, LAMA-FIRST, ENHSP-2020, OPTIC, TFD:
```bash
planutils install -y downward ff lama lama-first enhsp-2020 optic tfd
```

## PANDA
Panda is a planner based on Java. it can be donwloaded and run using the following commands:
1. Update the packages information and install java
```bash
sudo apt-get update
sudo apt install -y default-jre
```
2. Download PANDA planner in your workspace
```bash
cd Desktop/
wget https://www.uni-ulm.de/fileadmin/website_uni_ulm/iui.inst.090/panda/PANDA.jar
```

## PLANSYS2
<p align="justify">PlanSys2 is based on ROS2. To compile a PlanSys2 project you will need ROS2 and 2 more packages that are required to build the dependencies of the project (Rosdep) and to compile it (Colcon for ROS2). Follow the ensuing steps to install everything.
To compile a PlanSys2 project, you must have ROS2 installed, along with two additional packages necessary for building the project's dependencies (Rosdep) and compiling it (Colcon for ROS2). Follow the steps below to install all the required components.</p>

### ROS2
<p align="justify">Begin by installing ROS2. Make sure to follow the installation instructions specific to your operating system, which can be found on the official ROS2 website.
Once ROS2 is successfully installed, proceed to install Rosdep. Rosdep is a package manager for ROS that manages system dependencies required by ROS packages. Install Rosdep by executing the appropriate commands for your operating system. Detailed instructions can be found in the ROS documentation.</p>

1. Set locale and Setup sources
  ```bash
cd
locale
sudo apt update
sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
locale

sudo apt install -y software-properties-common
sudo add-apt-repository universe
# Press ENTER
  ```

2. Add the ROS 2 GPG key with apt and add the repository to your sources list, then install ROS-Base.
  ```bash
sudo apt update
sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null

sudo apt update
sudo apt upgrade

sudo apt install -y ros-humble-ros-base
  ```

### COLCON for ROS2
Get the files from the ROS2 repository and add its key to apt.
  ```bash
sudo sh -c 'echo "deb [arch=amd64,arm64] http://repo.ros2.org/ubuntu/main `lsb_release -cs` main" > /etc/apt/sources.list.d/ros2-latest.list'
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
sudo apt update
sudo apt install -y python3-colcon-common-extensions
  ```

### ROSDEP
Install rosdep.
  ```bash
sudo apt-get install -y python3-rosdep
sudo rosdep init
rosdep update
  ```

***
# Running the planners
<p align="justify">For running the planners on the domain and problem files of the assignment in this repository, you will need to run the following code:</p>

  ```bash
cd Desktop
git clone https://github.com/akaGallo/APTP_project
  ```

## PROBLEM 1
<p align="justify">To run the Problem 1, you will need the Planutils environment and the FastDownward planner for the usage of FF, LAMA, LAMA-FIRST and DOWNWARD.</p>

  ```bash
cd APTP_project/problem1
  ```
  
### SIMPLE
  ```bash
cd simple
# for each of the following wait the end of its execution
planutils run ff domain1_simple.pddl problem1_simple.pddl
planutils run lama domain1_simple.pddl problem1_simple.pddl
planutils run lama-first domain1_simple.pddl problem1_simple.pddl
planutils run downward domain1_simple.pddl problem1_simple.pddl "--search astar(goalcount)"
  ```

### CRANE
  ```bash
cd crane
# for each of the following wait the end of its execution
planutils run ff domain1_crane.pddl problem1_crane.pddl
planutils run lama domain1_crane.pddl problem1_crane.pddl
planutils run lama-first domain1_crane.pddl problem1_crane.pddl
planutils run downward domain1_crane.pddl problem1_crane.pddl "--search astar(goalcount)"
  ```

## PROBLEM 2
<p align="justify">To run the Problem 2, you have to use again FF, LAMA and LAMA-FIRST for the SIMPLE case, while for the FLUENTS case you will need the ENHSP-2020 planner.</p>

  ```bash
cd APTP_project/problem2
  ```
  
### SIMPLE
  ```bash
cd simple
# for each of the following wait the end of its execution
planutils run ff domain2_simple.pddl problem2_simple.pddl
planutils run lama domain2_simple.pddl problem2_simple.pddl
planutils run lama-first domain2_simple.pddl problem2_simple.pddl
  ```

### FLUENTS
  ```bash
cd fluents
# for each of the following wait the end of its execution
planutils run enhsp-2020 "-o domain2_fluents.pddl -f problem2_fluents.pddl -planner opt-blind"
planutils run enhsp-2020 "-o domain2_fluents.pddl -f problem2_fluents.pddl -planner opt-hmax"
planutils run enhsp-2020 "-o domain2_fluents.pddl -f problem2_fluents.pddl -planner opt-hrmax"
planutils run enhsp-2020 "-o domain2_fluents.pddl -f problem2_fluents.pddl -planner sat-aibr"
planutils run enhsp-2020 "-o domain2_fluents.pddl -f problem2_fluents.pddl -planner sat-hadd"
planutils run enhsp-2020 "-o domain2_fluents.pddl -f problem2_fluents.pddl -planner sat-hmrp"
planutils run enhsp-2020 "-o domain2_fluents.pddl -f problem2_fluents.pddl -planner sat-hmrph"
planutils run enhsp-2020 "-o domain2_fluents.pddl -f problem2_fluents.pddl -planner sat-hmrphj"
planutils run enhsp-2020 "-o domain2_fluents.pddl -f problem2_fluents.pddl -planner sat-hradd"
  ```

## PROBLEM 3
<p align="justify">To run Problem 3, make sure to have the PANDA planner available in your workspace. Load the PANDA planner before proceeding further.</p>

  ```bash
cd
cp Desktop/PANDA.jar Desktop/APTP_project/problem3/htn1
cp Desktop/PANDA.jar Desktop/APTP_project/problem3/htn2
cd Desktop/APTP_project/problem3
  ```
  
### HTN1
  ```bash
cd htn1
java -jar PANDA.jar -parser hddl domain3_htn1.hddl problem3_htn1.hddl
  ```

### HTN2
  ```bash
cd htn2
java -jar PANDA.jar -parser hddl domain3_htn2.hddl problem3_htn2.hddl
  ```

## PROBLEM 4
<p align="justify">To run the Problem 4, you will need the OPTIC planner.</p>

  ```bash
cd Desktop/APTP_project/problem4
  ```
  
### SIMPLE
  ```bash
cd simple
# for each of the following wait the end of its execution
planutils run optic "-N domain4_simple.pddl problem4_simple.pddl"
planutils run optic "-N -W1,1 domain4_simple.pddl problem4_simple.pddl"
planutils run optic "-N -E -W1,1 domain4_simple.pddl problem4_simple.pddl"
  ```

### FLUENTS
  ```bash
cd fluents
# for each of the following wait the end of its execution
planutils run optic "-N domain4_fluents.pddl problem4_fluents.pddl"
planutils run optic "-N -W1,1 domain4_fluents.pddl problem4_fluents.pddl"
planutils run optic "-N -E -W1,1 domain4_fluents.pddl problem4_fluents.pddl"
  ```

## PROBLEM 5
<p align="justify">To run PlanSys2, you need to have two separate terminals running simultaneously.</p>

  ```bash
cd Desktop/APTP_project/problem5
  ```
  
### SIMPLE
1. Open the TERMINAL 1 and run the following code:
<p align="justify">Proceed with the following steps in terminal one to build the dependencies, compile the project, and host the PlanSys2 planner based on ROS.</p>

  ```bash
cd plansys2_problem5_simple
bash terminal1.sh
  ```
  
2. Then, open the TERMINAL 2 and execute the final output:
<p align="justify">After setting up terminal 1, open a new terminal for terminal 2, that is dedicated to running the PlanSys2_terminal, which facilitates sending desired data (instances, predicates, goals) to the planner, computing a plan, and executing it.</p>

<p align="justify">NOTE: if the runninng is blocking because in terminal 1 appears this line "[ERROR] [plansys2_node-1]: process has died [pid 7203, exit code -11, cmd '/opt/ros/humble/lib/plansys2_bringup/plansys2_node --ros-args -r __ns:=/ --params-file /tmp/launch_params_uaitmls4 --params-file /opt/ros/humble/share/plansys2_bringup/params/plansys2_params.yaml'].", press Ctrl+C in both terminals and launch again the same code.</p>

  ```bash
cd Desktop/APTP_project/problem5/plansys2_problem5_simple
bash terminal2.sh
# [INFO] [...] [terminal]: No problem file specified.
# ROS2 Planning System console. Type "quit" to finish
```

3. Load the problem file.
  ```bash
source pddl/problem5_problem
```

4. Get the final plan.
  ```bash
get plan
```

5. Run it.
  ```bash
run
```

### FLUENTS
1. Open the TERMINAL 1 and run the following code:
  ```bash
cd Desktop/APTP_project/problem5/plansys2_problem5_fluents
bash terminal1.sh
  ```
  
2. Then, open the TERMINAL 2 and execute the final output:
  ```bash
cd Desktop/APTP_project/problem5/plansys2_problem5_fluents
bash terminal2.sh
# [INFO] [...] [terminal]: No problem file specified.
# ROS2 Planning System console. Type "quit" to finish
```

3. Load the problem file.
  ```bash
source pddl/problem5_problem
```

4. Get the final plan.
  ```bash
get plan
```

5. Run it.
  ```bash
run
```

***
# RESULTS
## Problem 1
### Simple
<img width="979" alt="p1-simple" src="https://github.com/akaGallo/APTP_project/assets/117358202/af92a684-eff2-4d71-a5d5-b229c3c7c5fb">

### Crane
<img width="990" alt="p1-crane" src="https://github.com/akaGallo/APTP_project/assets/117358202/c5fc1bde-6c99-49f4-91fa-3bdecbe073a4">

## Problem 2
### Simple
<img width="597" alt="p2-simple1" src="https://github.com/akaGallo/APTP_project/assets/117358202/8dfa6f7b-b9cb-49db-8b4e-cb8e0c0da1ae">
<img width="1007" alt="p2-simple" src="https://github.com/akaGallo/APTP_project/assets/117358202/e65913b1-3aa9-49d7-bc22-7c081d274b2a">

### Fluents
<img width="748" alt="sat-aibr" src="https://github.com/akaGallo/APTP_project/assets/117358202/a023fb27-4e2d-4208-90fa-984606d46d9d">
<img width="737" alt="sat" src="https://github.com/akaGallo/APTP_project/assets/117358202/c5e76508-0e86-4eb1-9cc2-e6980c332cfb">
<img width="804" alt="opt" src="https://github.com/akaGallo/APTP_project/assets/117358202/e1be6ab5-77a7-47cc-8d76-dd9319f165a4">

## Problem 3
### Htn 1
<img width="454" alt="htn1" src="https://github.com/akaGallo/APTP_project/assets/117358202/a0a04334-8f67-4a6a-9554-9a7ec968e790">

### Htn 2
<img width="454" alt="htn2" src="https://github.com/akaGallo/APTP_project/assets/117358202/867554ab-3e38-416f-a933-5db6bca9a5df">

## Problem 4
### Simple
<img width="1002" alt="p4-simple" src="https://github.com/akaGallo/APTP_project/assets/117358202/729dd9c9-d7f3-4ad0-a5b4-9dcb79156c6f">

### Fluents
<img width="985" alt="p4-fluents" src="https://github.com/akaGallo/APTP_project/assets/117358202/fcc837e6-d16b-41e4-8f58-505b6577418c">

## Problem 5
### Simple
<img width="1023" alt="p5-simple" src="https://github.com/akaGallo/APTP_project/assets/117358202/f08b72b2-05a8-442c-9c6c-ffbac638fe7f">

### Fluents
<img width="1011" alt="p5-fluents" src="https://github.com/akaGallo/APTP_project/assets/117358202/e3e10f75-6a2a-436c-a3a5-b545c4263c7e">
