# Introduction à Spark

## Lancement d'un notebook Jupyter/Spark avec docker
Le [projet Jupyter](http://jupyter.org/) propose un ensemble d'[images docker](https://hub.docker.com/u/jupyter/) ([github](https://github.com/jupyter/docker-stacks)).
En particulier, l'image [all-spark-notebook](https://hub.docker.com/r/jupyter/all-spark-notebook/) intègre Spark et Jupyter.

```bash
docker run -it --rm -p 8888:8888 -v `pwd`:/home/jovyan jupyter/all-spark-notebook
```

