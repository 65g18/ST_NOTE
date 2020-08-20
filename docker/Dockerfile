FROM debian:10.3

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8
ENV PATH /opt/conda/bin:$PATH

RUN apt-get update --fix-missing && \
    apt-get install -y wget bzip2 ca-certificates curl git && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Installation of Miniconda
RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-py38_4.8.2-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /opt/conda && \
    rm ~/miniconda.sh && \
    /opt/conda/bin/conda clean -tipsy && \
    ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate base" >> ~/.bashrc

# Installation of Tini
ENV TINI_VERSION v0.19.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /usr/bin/tini
RUN chmod +x /usr/bin/tini

RUN conda install -y conda

# Installation of packages
RUN conda install -y \
  numpy \
  scipy \
  scikit-learn \
  jupyter \
  notebook \
  ipython

# installation additional packages
ADD ./requirements.txt requirements.txt
RUN pip install -U pip && pip install -r requirements.txt

# remove cache files
RUN apt-get autoremove -y && apt-get clean && \
  conda clean -i -t -y  && \
  rm -rf /usr/local/src/*

WORKDIR /book/
