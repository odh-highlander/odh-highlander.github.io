# Open Data Hub with Easybuild and Lmod, or thousands of libraries and applications without installation!

![ODH+EasyBuild+Lmod](./img/banner.png)

!!! note "Important, please read"

    This project is a work in progress to properly refactor the initial code, which is meanwhile availablein different repos, [here](https://github.com/guimou/odh-highlander), [here](https://github.com/guimou/s2i-lmod-notebook) and [here](https://github.com/guimou/jupyter-lmod)

**TL;DR:** container images on a data science platform like [Open Data Hub](http://opendatahub.io/) are hard to manage if you want to provide users with many different libraries or applications, moreover at various versions.
This repo shows how you can use EasyBuild, Lmod and a shared PVC to bring thousands of those instantly in the notebook environments with a single container image! (Therefore the name of the project, "There can be only one…​")

## The problem

If you manage shared data science environments based on Kubernetes, like [Open Data Hub](http://opendatahub.io/) on [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift), you may face a container image management problem. That is especially true with interactive environments like Jupyter-as-a-Service. Your users will want to use various Jupyter images that include this or this library or application, at this or this version. Of course it’s clearly impossible to create behemoth images including each and every library you’re asked for, or to create and manage hundreds (thousands?) of variations of handpicked selection of libraries and apps to fit a specific need. Granted, you could let the users pip or conda install everything from scratch every time they launch the environment. But it’s not that practical, not all packages are available through this mean, especially your own private applications. Moreover, in an enterprise environment, the as-a-service container won’t run as root, so no package can be yum or apt-get installed.

![Your options](img/container_images.png)

## The solution

n this repo you will find tools and instructions for a different approach to this problem, thanks to the HPC community who has solved it a long time ago! This is kind of a portage of their solution to Kubernetes.

The idea is that instead of building the needed applications and libraries into the container images, you can store all of them in a read-only shared library on a ROX volume (read-only, but by many pods simultaneously), and mount this volume inside the Pods at spawn time.
Then, leveraging Linux Environment Modules with [Lmod](https://lmod.readthedocs.io/en/latest/), it gets easy to dynamically "load" what you need, when you need it, instantly…​ Pretty neat, uh?

![Dynamic Loading](img/dynamic_loading.png)

And with [Easybuild](https://easybuild.io/), you have access to a few thousands of ready to use (or more accurately to compile, we’ll come to that) software packages, plus the full machinery to easily create you owns.

## What does it look like?

With the help of a great Jupyter extension to easily load those shared modules, it’s really easy! Have a look:

![Dynamic Loading](img/jupyter-lmod.gif)
