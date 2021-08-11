# Software Practices

Most of what we do in this research group is about, or enabled by, open-source software - so it is really good to get on top of practices and workflow for doing this.

Most software development will be done in Python, and I recommend using Anaconda to install Python 3 and pip to manage packages. 

## My GitHub

All my code is version-controlled [github.com/benjaminpope](https://github.com/benjaminpope/) - you should set up yours! 

## Making Open-Source Software

Christina Hedges (Ames) has a fantastic introduction to open-source software practices for astronomy, which I won't try to compete with:

- [christinahedges.github.io/astronomy_workflow](https://christinahedges.github.io/astronomy_workflow/)

## Autodiff

We will rely extensively on automatic differentiation, or autodiff. Like magic, we can propagate derivatives through almost arbitrary code via the chain rule - which means we can do fast, gradient-based inference, optimization, and perturbation theory.

Here are some great tutorials:

- [The Google Jax Autodiff Cookbook](https://jax.readthedocs.io/en/latest/notebooks/autodiff_cookbook.html)
- In the context of Dan Foreman-Mackey's code exoplanet: [link](https://docs.exoplanet.codes/en/latest/tutorials/autodiff/)
- This great explainer of autodiff by Robert Lange: [link](https://towardsdatascience.com/forward-mode-automatic-differentiation-dual-numbers-8f47351064bf)