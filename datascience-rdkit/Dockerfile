FROM douglasconnect/datascience-notebook

RUN conda install --yes --freeze-installed \
    r-pheatmap \
    r-reshape2 \
    r-gplots \
    r-rcolorbrewer \
    r-biocmanager \
    r-drc \
    r-mass \
  && conda install --yes --freeze-installed -c rdkit rdkit \
  && R -e 'BiocManager::install("DESeq2")' \
  && conda clean -afy \
  && find /opt/conda/ -follow -type f -name '*.a' -delete \
  && find /opt/conda/ -follow -type f -name '*.pyc' -delete \
  && find /opt/conda/ -follow -type f -name '*.js.map' -delete \
  && fix-permissions $CONDA_DIR \
  && fix-permissions /home/$NB_USER \
  && fix-permissions /etc/jupyter/
