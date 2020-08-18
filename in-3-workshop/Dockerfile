FROM douglasconnect/datascience-notebook:0.1

USER root

# R pre-requisites
RUN apt-get clean && \
    apt-get update --allow-unauthenticated && \
    apt-get install -y --no-install-recommends \
        gfortran && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*
# we make /etc/password writeable by group so that when we run as non-root user it can be added and we get a username

USER $NB_USER

ADD .condarc .condarc

RUN conda install --yes --freeze-installed \
    r-biocmanager \
    bioconductor-deseq2 \
    r-drc \
    r-mass \
    r-pheatmap \
    r-gplots \
    rdkit \
  && conda clean -afy \
  && find /opt/conda/ -follow -type f -name '*.a' -delete \
  && find /opt/conda/ -follow -type f -name '*.pyc' -delete \
  && find /opt/conda/ -follow -type f -name '*.js.map' -delete \
  && fix-permissions $CONDA_DIR \
  && fix-permissions /home/$NB_USER \
  && fix-permissions /etc/jupyter/
