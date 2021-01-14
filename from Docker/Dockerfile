# Choose your desired base image
FROM jupyter/minimal-notebook:latest


# name your environment and choose python 3.x version
ARG conda_env=python3
ARG py_ver=3.6
ARG tf_ver=1.15.0

# you can add additional libraries you want conda to install by listing them below the first line and ending with "&& \"
# Python vs jupyter conflict fix (https://stackoverflow.com/questions/50675004/conflicting-python-version-in-jupyter-notebookconda)
# RUN conda create --quiet --yes -p $CONDA_DIR/envs/$conda_env python=$py_ver ipython jupyterlab ipykernel htop cudatoolkit=11.0 && \
#     conda clean --all -f -y

RUN conda create --quiet --yes -p $CONDA_DIR/envs/$conda_env python=$py_ver ipython jupyterlab ipykernel htop && \
    conda clean --all -f -y

# alternatively, you can comment out the lines above and uncomment those below
# if you'd prefer to use a YAML file present in the docker build context

# COPY --chown=${NB_UID}:${NB_GID} environment.yml /home/$NB_USER/tmp/
# RUN cd /home/$NB_USER/tmp/ && \
#     conda env create -p $CONDA_DIR/envs/$conda_env -f environment.yml && \
#     conda clean --all -f -y


# create Python 3.x environment and link it to jupyter
RUN $CONDA_DIR/envs/${conda_env}/bin/python -m ipykernel install --user --name=${conda_env} && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

# any additional pip installs can be added by uncommenting the following line
RUN $CONDA_DIR/envs/${conda_env}/bin/pip install tensorflow==${tf_ver} tensorflow-gpu==${tf_ver}

# prepend conda environment to path
ENV PATH $CONDA_DIR/envs/${conda_env}/bin:$PATH

# if you want this environment to be the default one, uncomment the following line:
ENV CONDA_DEFAULT_ENV ${conda_env}

# # Install additional software but not working because of permissions 
# RUN apt-get install -y \
#     htop \
#     vim \
#     screen