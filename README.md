# abb_driver

[![Build Status](http://build.ros.org/job/Mdev__abb_driver__ubuntu_bionic_amd64/badge/icon)](http://build.ros.org/job/Mdev__abb_driver__ubuntu_bionic_amd64)
[![Build Status: Travis CI](https://travis-ci.com/ros-industrial/abb_driver.svg?branch=kinetic-devel)](https://travis-ci.com/ros-industrial/abb_driver)
[![Github Issues](https://img.shields.io/github/issues/ros-industrial/abb_driver.svg)](http://github.com/ros-industrial/abb_driver/issues)

[![license - bsd 3 clause](https://img.shields.io/:license-BSD%203--Clause-blue.svg)](https://opensource.org/licenses/BSD-3-Clause)

[![support level: community](https://img.shields.io/badge/support%20level-community-lightgray.png)](http://rosindustrial.org/news/2016/10/7/better-supporting-a-growing-ros-industrial-software-platform)

[ROS-Industrial][] RAPID based driver for ABB IRC5 controllers.


## Contents

This repository contains a simple, RAPID based ROS driver for ABB industrial robots connected to IRC5 controllers.

The driver is largely manipulator agnostic, and is expected to work with any ABB manipulator compatible with an IRC5 controller.

For more information, refer to the [ROS wiki][].

Note: this is not a new development, but a migration of the old `abb_driver` package to a separate repository. See [Origin and history](#origin-and-history) for more information.


## TOC

1. [Status](#status)
1. [Performance](#performance)
1. [Requirements](#requirements)
1. [Releases and supported ROS distributions](#releases-and-supported-ros-distributions)
1. [Installation](#installation)
1. [Building from source](#building-from-source)
1. [Usage](#usage)
1. [Origin and history](#origin-and-history)


## Status

This package is usable as-is, but is not feature complete.
Only joint motion can be commanded and there is no support for IO, Cartesian motion or any other more advanced RAPID functionality.

No significant development is planned, as focus has shifted to [abb_robot_driver][] (with [abb_libegm][] and [abb_librws][]).

Community contributed usability enhancements and new features will however be accepted and merged.


## Performance

Performance of the driver (of both the ROS and RAPID components) is deemed sufficient for mildly dynamic workloads (ie: pick-and-place and relatively slow motion).
Processes for which control of the robot's position and velocity must achieve high resolution in both time and space are not supported by this driver.

Users are encouraged to consider using [abb_libegm][] and [abb_librws][] instead.


## Requirements

Please refer to the [ROS wiki][] for information on supported controllers, required RobotWare versions and required controller options.


## Releases and supported ROS distributions

This repository follows the main [abb][] repository as far as development and maintenance policies, branching strategies and release scheduling.
Branch naming follows the ROS distribution they are compatible with. `-devel` branches may be unstable.

`abb_driver` has been successfully built from sources with both ROS Kinetic and ROS Melodic, on Ubuntu Xenial and Ubuntu Bionic respectively.
Only `amd64` architectures have been tested.
Other combinations of OS, ROS versions and architectures may work, but have not been tested.

Binary packages are provided for ROS Kinetic and ROS Melodic on all platforms supported by the ROS buildfarm for those ROS releases.
Refer to [REP 3: Target Platforms][], *Kinetic Kame* and *Melodic Morenia* sections for more information on which platforms are supported.


## Installation

On Ubuntu, installation via `apt` is recommended over building the package from source.

For ROS Melodic, the following command installs the driver and all of its dependencies (after having configured the ROS package repositories):

```
sudo apt install ros-melodic-abb-driver
```

When using ROS Kinetic, replace `melodic` with `kinetic`.

Instructions to build the driver from source, refer to the next section.


## Building from source

### On newer (or older) versions of ROS

Building `abb_driver` on Ubuntu Xenial/ROS Kinetic and Ubuntu Bionic/ROS Melodic systems is supported. This will require creating a Catkin workspace, cloning this repository, installing all required dependencies and finally building the workspace.

### Catkin tools

It is recommended to use [catkin_tools][] instead of the default [catkin][] when building ROS workspaces. `catkin_tools` provides a number of benefits over regular `catkin_make` and will be used in the instructions below. The package can be built using `catkin_make` however: use `catkin_make` in place of `catkin build` where appropriate.

### Building the package

The following instructions assume that a [Catkin workspace][] has been created at `$HOME/catkin_ws` and that the *source space* is at `$HOME/catkin_ws/src`. Update paths appropriately if they are different on the build machine.

These instructions build the `kinetic-devel` branch on a ROS Kinetic system:

```bash
# change to the root of the Catkin workspace
cd $HOME/catkin_ws

# retrieve the latest development version of the abb_driver repository. If you'd rather
# use the latest released version, replace 'kinetic-devel' with 'kinetic'
git clone -b kinetic-devel https://github.com/ros-industrial/abb_driver.git src/abb_driver

# check for and install missing build dependencies.

# first: update the local database
rosdep update

# now install dependencies, again using rosdep.
# Note: this may install additional packages, depending on the software already present
# on the machine.
# Be sure to change 'kinetic' to whichever ROS version you are using
rosdep install --from-paths src/ --ignore-src --rosdistro kinetic

# build the workspace (using catkin_tools)
catkin build
```

### Activating the workspace

Finally, activate the workspace to get access to the packages just built:

```bash
source $HOME/catkin_ws/devel/setup.bash
```

At this point the package should be usable (ie: `roslaunch` should be able to auto-complete `abb_driver`). In case the workspace contains additional packages (ie: not from this repository), those should also still be available.


## Usage

Refer to [Working With ROS-Industrial Robot Support Packages][] for information on how to use the files provided by the driver. See also the other pages on the [ROS wiki][].

Refer to the [tutorials][] for information on installation and configuration of the controller-specific software components.


## Origin and history

This package was extracted from the main [abb][] repository.
Refer to [ros-industrial/abb#179][] for rationale and a description of the workflow.

As `ros-industrial/abb` contained fairly large files which would no longer be relevant for the extracted package, `git-filter-branch` was used to prune everything unrelated to the driver. This has rewritten history.

Commits and file provenance however have been retained as much as possible, and references to issues and PRs have been updated wherever possible.
Only the `kinetic-devel` branch was migrated: others can still be found at `ros-industrial/abb`.
Open issues on the tracker of `ros-industrial/abb` against `abb_driver` have been transferred to the new repository.

Note: as commit history has been altered, commit hashes will have changed.
References in issues and comments on commits in this repository from before the migration will therefor no longer link to their respective commits.






[ROS-Industrial]: http://wiki.ros.org/Industrial
[ROS wiki]: http://wiki.ros.org/abb_driver
[abb]: https://github.com/ros-industrial/abb
[ros-industrial/abb#179]: https://github.com/ros-industrial/abb/issues/179
[abb_robot_driver]: https://github.com/ros-industrial/abb_robot_driver
[abb_libegm]: https://github.com/ros-industrial/abb_libegm
[abb_librws]: https://github.com/ros-industrial/abb_librws
[REP 3: Target Platforms]: https://ros.org/reps/rep-0003.html
[Catkin workspace]: http://wiki.ros.org/catkin/Tutorials/create_a_workspace
[catkin]: http://wiki.ros.org/catkin
[catkin_tools]: https://catkin-tools.readthedocs.io/en/latest
[Working With ROS-Industrial Robot Support Packages]: http://wiki.ros.org/Industrial/Tutorials/WorkingWithRosIndustrialRobotSupportPackages
[tutorials]: http://wiki.ros.org/abb/Tutorials

