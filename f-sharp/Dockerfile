FROM jupyter/datascience-notebook:latest
MAINTAINER Daniel Bachler <daniel@douglasconnect.com>

USER root
RUN apt-get update && \
    apt-get install -y build-essential fish openssh-client && \
    rm -rf /var/lib/apt/lists/*
RUN wget -q https://packages.microsoft.com/config/ubuntu/20.04/packages-microsoft-prod.deb -O packages-microsoft-prod.deb \
	&& dpkg -i packages-microsoft-prod.deb \
	&& rm packages-microsoft-prod.deb \
	&& apt-get update \
	&& apt-get install -y software-properties-common \
    && add-apt-repository universe \
	&& apt-get update \
	&& apt-get install -y apt-transport-https \
	&& apt-get update \
	&& apt-get install -y dotnet-sdk-3.1

USER $NB_UID

ENV PATH="~/.dotnet/tools:${PATH}"
ENV DOTNET_RUNNING_IN_CONTAINER=true \
    # Enable correct mode for dotnet watch (only mode supported in a container)
    DOTNET_USE_POLLING_FILE_WATCHER=true \
    # Skip extraction of XML docs - generally not useful within an image/container - helps performance
    NUGET_XMLDOC_MODE=skip \
    # Opt out of telemetry until after we install jupyter when building the image, this prevents caching of machine id
    DOTNET_TRY_CLI_TELEMETRY_OPTOUT=true
RUN dotnet tool install --global Microsoft.dotnet-interactive
RUN dotnet interactive jupyter install \
	&& jupyter kernelspec list
RUN fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER
RUN conda install rdkit altair && \
    jupyter labextension install @jupyterlab/git && \
    pip install jupyterlab-git nbdime && \
    jupyter labextension install nbdime-jupyterlab && \
    jupyter serverextension enable --py --sys-prefix jupyterlab_git && \
    conda clean -tipsy && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER



RUN rm -rf /opt/conda/lib/R/library/haven
RUN R --slave -e 'withr::with_makevars(c(PKG_LIBS = "-liconv"), install.packages("haven", repos="http://cran.us.r-project.org"), assignment = "+=")'
RUN R --slave -e 'withr::with_makevars(c(PKG_LIBS = "-liconv"), install.packages("readxl", repos="http://cran.us.r-project.org"), assignment = "+=")'
RUN R --slave -e 'install.packages(c("RJSONIO", "drc"), repos="http://cran.us.r-project.org")'
