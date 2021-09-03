The Jax team ran out of space on PyPI so they are now semi-regularly deleting old versions of jax and jaxlib from pip. To find current supported versions, use this [jaxlib · PyPI](https://pypi.org/project/jaxlib/#history) link.

They discuss this on the github discussions page: [jaxlib removal from pypi · Discussion #7608 · google/jax · GitHub](https://github.com/google/jax/discussions/7608) details the removal, and provides basic instructions about how to install depreciated versions.

To install old versions first navigate to [this](https://storage.googleapis.com/jax-releases/jax_releases.html) website and find the relvelant version. Note the cuda versions include GPU support, while the nocuda version is the standard version. Say we want version 0.1.57, there are multiple distributions:

> nocuda/jaxlib-0.1.57-cp36-none-manylinux2010_x86_64.whl
> nocuda/jaxlib-0.1.57-cp37-none-manylinux2010_x86_64.whl
> nocuda/jaxlib-0.1.57-cp38-none-manylinux2010_x86_64.whl
> nocuda/jaxlib-0.1.57-cp39-none-manylinux2010_x86_64.whl

Since we don't know which one we have to try all of them. Install to your venv like so:

```
pip install https://storage.googleapis.com/jax-releases/nocuda/jaxlib-0.1.57-cp38-none-manylinux2010_x86_64.whl
```

cp38 turns out the be the correct version for OSX, with the other versions throwing an error:

```
ERROR: jaxlib-0.1.57-cp39-none-manylinux2010_x86_64.whl is not a supported wheel on this platform
```

On the github discussion page they also show how to update requirements.txt files to search this google storage drive for the required version. For exmaple this would be how to install the correct jax versions for the present (as of 02/09/2021) stable Morphine distribution

```
# jax
--find-links https://storage.googleapis.com/jax-releases/jax_releases.html
jax==0.1.77
jaxlib==0.1.57
```
