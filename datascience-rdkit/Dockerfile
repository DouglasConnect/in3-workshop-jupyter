FROM douglasconnect/datascience-notebook:0.1

USER $NB_USER

RUN conda install --yes --freeze-installed rdkit \
  && conda clean -afy \
  && find /opt/conda/ -follow -type f -name '*.a' -delete \
  && find /opt/conda/ -follow -type f -name '*.pyc' -delete \
  && find /opt/conda/ -follow -type f -name '*.js.map' -delete \
  && fix-permissions $CONDA_DIR \
  && fix-permissions /home/$NB_USER \
  && fix-permissions /etc/jupyter/
