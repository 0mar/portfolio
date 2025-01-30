---
title:  "Role-based access control in JupyterHub: completed"
date: 2021-09-05
tags: programming python jupyter
---

The [JupyterHub](https://jupyter.org/hub) RBAC project has come to an end! 
Although there are still some creases to be ironed out, we have an implementation that will be rolled out with JupyterHub 2.0. 
It was an incredibly fun project to work on, and a big thank you to [@minrk](https://github.com/minrk), [@IvanaH8](https://github.com/IvanaH8), [@consideRatio](https://github.com/consideRatio), [@manics](https://github.com/manics) and the entire Jupyter team for all their support and knowledge. 
Read on for a brief description of what we set out to do (originally posted on my previous [blog](https://symbols.hotell.kau.se)), or take a look at the [docs](https://jupyterhub.readthedocs.io/en/rbac/rbac/).

<!--more-->

## What is JupyterHub

In a way, JupyterHub is an extension of Jupyter Notebook, allowing you to setup a server with a programming environment that can be accessed by any user with a browser and an internet connection.
Consider the following example. You teach a course in computational science, and you want your students to work with a scientific software stack in Python. Python and its popular scientific computing libraries are all freely available, so reasonably you can expect your students to install them. However, without experience, doing so can be a pain, especially on proprietary software systems. In my experience, it can take an entire lab session just to get everyone up and running with a working environment.

Other options exist, like prepared publicly available computers, installing virtual machines or using a third-party cloud service that can execute Python code.
However, each of these options comes with either a significant time or money investment. JupyterHub can help here. By dedicating one or more machines to run a JupyterHub server, the Python environment is setup and maintained centrally. Your students login, and they are good to go.

## The JupyterHub API

JupyterHub comes with an interface that allows its maintainer to perform administrative actions like adding and removing users, and starting and stopping individual Jupyter servers.
This admin interface relies on the JupyterHub API, which communicates the actions to the hub itself.
The API relies on authentication to determine if actions are allowed. For instance, currently the default behaviour is that creating or deleting users requires so-called admin rights. This ensures that an arbitrary user, or even an unauthenticated third party, cannot disrupt the status of the Hub.

This system is functional, but lacks flexibility or scalability. Imagine, for instance, that you're dealing with a large set of users, each belonging to a number of groups. With this framework, appointing a 'group admin', a user that has admin rights pertaining to actions of only their group, requires full rights to all actions. Implementing a bot that shuts down idle servers and saves tons of resources, idem dito.

Implementing RBAC will remedy situations like this. By equipping services, groups and users with *roles* that supply them with a collection of permissions, administrators are able to fine-tune which parties are able to access which resources.

You can follow all JupyterHub development activity in the GitHub [repository](https://github.com/jupyterhub/jupyterhub).


