FROM jupyter/datascience-notebook:7e2f380b1792

USER root

# R pre-requisites
RUN apt-get clean && \
    mv /var/lib/apt/lists /tmp && \
    mkdir -p /var/lib/apt/lists/partial && \
    apt-get clean && \
    apt-get update --allow-unauthenticated && \
    apt-get install -y --no-install-recommends \
        fonts-dejavu \
        build-essential && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    chmod g+w /etc/passwd
# we make /etc/password writeable by group so that when we run as non-root user it can be added and we get a username

USER $NB_USER

RUN conda install --yes --freeze-installed \
    pandas \
    requests \
    simplejson \
    ipywidgets \
    seaborn \
    matplotlib \
    altair \
    r-rjsonio \
    r-tidyverse \
  && pip install --upgrade edelweiss-data jupyterlab-git==0.20.0 \
  && jupyter lab build \
  && conda clean -afy \
  && find /opt/conda/ -follow -type f -name '*.a' -delete \
  && find /opt/conda/ -follow -type f -name '*.pyc' -delete \
  && find /opt/conda/ -follow -type f -name '*.js.map' -delete \
  && fix-permissions $CONDA_DIR \
  && fix-permissions /home/$NB_USER \
  && fix-permissions /etc/jupyter/
