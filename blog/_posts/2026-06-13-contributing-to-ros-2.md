---
layout: post
title: Contributing to the ROS 2 project
subtitle: How-to guide
# gh-repo: ilarioazzollini/autonomous-unicycle
# gh-badge: [star, fork, follow]
cover-img: /blog/img/molveno.jpg
thumbnail-img: /blog/img/rolling_transparent.png
share-img: /blog/img/path.jpg
tags: [ros, programming, open source]
comments: false
---

If you're reading this, I assume you are curious about how to contribute to the [Robot Operating System (ROS)](https://www.ros.org/) project, and you are probably a user already. Like me, you've thought about getting more involved with the community, but you're not sure where to start or how much effort it takes. If that sounds familiar, then this guide should help.

Before we dive in, I want to spend a moment on *why* contributing to open source is worth your time. I'm just starting out, so I cannot lecture anyone, but I'd simply like to share what's driving me. The first reason may be obvious: you're giving back to a project that you and thousands of others rely on, being actively part of something greater. Every improvement you make will run on real robots around the world, on systems you cannot even imagine, solving real problems. That's a genuinely cool thing to be part of. The second reason is more personal: contributing to a mature, well-maintained project like ROS 2 is one of the best ways to grow as an engineer. The maintainers are experienced, thoughtful people, and getting mentored by them (discussing design choices and implementation plans, and getting your code reviewed) is a rare learning opportunity. So, you're helping the world, but you're also learning and improving along the way.

Rather than handing you a list of instructions or links to ROS 2 documentation, I want to take you along on my own journey to my first code contribution to the ROS 2 project. We'll learn by doing, and this guide will be as hands-on as possible. In fact, I'm working on this contribution as I write. Specifically, we'll be contributing to the [Rolling Ridley](https://docs.ros.org/en/rolling/Releases/Release-Rolling-Ridley.html) distribution, following the [Contributing](https://docs.ros.org/en/rolling/The-ROS2-Project/Contributing.html) page (which I recommend reading after this guide).

{: .box-note}
**Note:** The main goal of this guide is to convince you that getting started really isn't that hard, and that the ROS 2 community is genuinely helpful and responsive. All you need to do is show up and, when in doubt, ask. Good places to do that include the [Open Robotics Discourse](https://discourse.openrobotics.org/), the [Robotics Stack Exchange](https://robotics.stackexchange.com/), and the [Open Robotics Zulip Server](https://openrobotics.zulipchat.com/). All other developer resources can be found at [https://docs.ros.org/](https://docs.ros.org/).

## 1. Find something interesting to work on

First of all, we need to find something interesting to work on. I'm comfortable writing C++ and have used [rclcpp](https://github.com/ros2/rclcpp), that is *the ROS Client Library for C++*, for quite a while. Let's head to [its open issues](https://github.com/ros2/rclcpp/issues) and see what's there.

![rclcpp issues](/blog/img/rclcpp_issues.png){: .mx-auto.d-block :}

Each issue has one or more labels attached, usually added by maintainers to help people filter and navigate them.

The first thing to know is that as soon as a new issue is opened (or shortly after), it gets tagged as a `bug`, an `enhancement`, or `documentation`, giving us an early sense of what kind of work is involved.

[Two other labels worth knowing about](https://docs.ros.org/en/rolling/The-ROS2-Project/Contributing.html#what-to-work-on) are `help wanted` and `good first issue`. The former marks issues that maintainers think are suitable for community members to pick up. The latter flags issues considered more accessible for first-time contributors. Filtering by these is always a good starting point.

One more thing to watch for when choosing an issue, especially if you're still finding your footing, is how *mature* the discussion is. Let me elaborate. When someone opens an issue, one or more maintainers will weigh in, starting a design discussion. The goal is first to understand the problem and decide whether a change is worth pursuing, and then — if the answer is yes — to agree on the best way to implement it. Maintainers often provide detailed guidelines and expected behavior at this stage. Once that happens, the issue is mature enough to work on safely: there's already an implementation idea, and the maintainers can easily guide you because the solution is clear in their heads.

Now, I don't feel like fixing bugs at the moment (it would feel too much like real work). So, I'm looking for an `enhancement`, with or without `help wanted`, preferably where maintainers have already aligned on a concrete design plan.

And the winner is [issue 2981](https://github.com/ros2/rclcpp/issues/2981): a request for a new API that lets users save and load all parameters of a specific node to and from a YAML file. It's interesting to me because the maintainers have already agreed on an approach, and the feature touches both [rcl](https://github.com/ros2/rcl) and [rclcpp](https://github.com/ros2/rclcpp) on the [rolling development branch](https://docs.ros.org/en/rolling/Releases/Release-Rolling-Ridley.html).

To make it official, the next step is to join the discussion and confirm nobody else is already working on it.

![rclcpp issues](/blog/img/issue2981_confirmation.png){: .mx-auto.d-block :}

TODO: open the corresponding issue also on rcl and get confirmation for the design idea.

And it is on!

## 2. Setup the development environment

The goal is to start developing as soon as possible, with an environment that works seamlessly on Linux, macOS, and Windows and relies on as few system dependencies as possible, all of which should be free to use, at least for non-commercial purposes. For these reasons, we will rely on [Docker](https://www.docker.com/) to containerize our development environment. This approach ensures that everyone works in the same reproducible environment, regardless of their host operating system.

The machine on which Docker is installed, and from which we build Docker images and run Docker containers, will be referred to as the *host PC*. Regardless of the host PC’s operating system, all development work in this guide will take place inside an Ubuntu Docker container. This is because Ubuntu is at the core of the official [Open Source Robotics Foundation (OSRF) Docker images](https://github.com/osrf/docker_images).

{: .box-warning}
**Warning:** This holds as long as our work does not involve graphics and graphical user interfaces. Allowing a container to access and use the display of the host PC is not always straightforward: it is easy to share the display when the host PC runs Linux, but more involved on a Windows or macOS host PC.

### 2.1 Prerequisites

The following tools are required:

- [Git](https://git-scm.com/)
- [Docker](https://www.docker.com/)
- [VS Code](https://code.visualstudio.com/) (optional, but recommended for a fully integrated development environment)

After installing these tools (and ensuring they are available in your system `PATH`), open a terminal or PowerShell and run the following commands:

```bash
git --version
docker --version
code --version
```

If the installation was successful, each command should print the installed version without errors.

### 2.2 The `ros-dev` repository

To streamline development, I've created the [ros-dev repo](https://github.com/ilarioazzollini/ros-dev), which you can fork and clone so we can start building right away.

Let us follow its main [README.md](https://github.com/ilarioazzollini/ros-dev/blob/main/README.md) file together.

Open a terminal, navigate to the directory where you want to clone this repository, and clone it with the following command (or clone your personal fork instead):

```bash
git clone git@github.com:ilarioazzollini/ros-dev.git
cd ros-dev
```

{: .box-note}
**Note:** From this point on, **all terminal commands in this guide should be executed from the root directory of this repository unless otherwise specified**.

Now we can build the Docker container image by running:

```bash
docker build --platform linux/amd64 -f docker/Dockerfile -t ros-rolling-dev .
```

Then run a container from the image we just built:

```bash
docker run --platform linux/amd64 -it --rm --privileged --network=host --ipc=host -v ${PWD}:/root/ros-dev -w /root/ros-dev --name ros-dev-container ros-rolling-dev bash
```

We should now be inside a terminal session running in the Docker container, and we can confirm that it is an Ubuntu container by running:

```bash
lsb_release -a
```

For now, let us simply exit, which shuts down and automatically removes the container:

```bash
exit
```

### 2.3 Preparing the needed ROS 2 repos

As already mentioned, we will work on [rcl](https://github.com/ros2/rcl) and [rclcpp](https://github.com/ros2/rclcpp). The first step is to fork both of them. If you have already forked them, sync your forks with the upstream repos instead.

In my case, I had already forked rclcpp from an earlier contribution, so I just need to sync it. That's easy: I only have to click *Sync fork* and then *Update branch*.

![rclcpp sync](/blog/img/rclcpp_sync.png){: .mx-auto.d-block :}

After that, I can clone it into the `ros-dev/repos` folder:

```bash
cd ros-dev/repos/
git clone git@github.com:ilarioazzollini/rclcpp.git
```

check that it is on the `rolling` branch:

```bash
cd rclcpp
git status
```

and immediately create a new branch to work on the issue:

```bash
git checkout -b ilo/issue-2981
```

For rcl, I do the same, except that this time I start by creating the fork.

![rcl fork](/blog/img/rcl_fork.png){: .mx-auto.d-block :}

Be sure to uncheck the "Copy the `rolling` branch only" checkbox, since we may later want to backport the changes to other ROS 2 distributions as well.

![rcl fork all branches](/blog/img/rcl_fork_all_branches.png){: .mx-auto.d-block :}

>TODO: create the corresponding branch, once we have an issue number on the rcl repo
>```bash
>git checkout -b ilo/issue-2981
>```

### 2.4 Building the ROS 2 workspace

Going back to the `ros-dev` [README.md](https://github.com/ilarioazzollini/ros-dev/blob/main/README.md) file, we can now set up the workspace.

First, let us start the Docker container again (or the [VS Code DevContainer](https://github.com/ilarioazzollini/ros-dev/blob/main/README.md#optional-working-with-vs-code-1)) and link the repos we just cloned into `ros-dev/ros2_ws/src/` (everything under this folder will be built and tested):

```bash
ln -s /root/ros-dev/repos/rcl /root/ros-dev/ros2_ws/src/rcl

ln -s /root/ros-dev/repos/rclcpp /root/ros-dev/ros2_ws/src/rclcpp
```

Now we can use the [convenience scripts](https://github.com/ilarioazzollini/ros-dev/tree/main/scripts) to build and test the whole workspace. Since this is the first time, let us use the following one:

```bash
bash /root/ros-dev/scripts/clean_build_test.sh
```

After a while, we should have successfully built both `rcl` and `rclcpp` (tests included!).

# 3. Working on the issue

TODO: TBD...