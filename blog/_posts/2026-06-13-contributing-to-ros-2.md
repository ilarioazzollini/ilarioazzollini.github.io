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

And it is on!

## 2. Setup the development environment

TBD...
