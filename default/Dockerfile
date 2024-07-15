FROM python:3.8-slim

WORKDIR /app

# install dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends bash wget procps && \
    rm -rf /var/lib/apt/lists/* && \
    wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O /tmp/miniconda.sh && \
    bash /tmp/miniconda.sh -b -p /miniconda && \
    rm /tmp/miniconda.sh && \
    /miniconda/bin/conda clean -t -i -p -y && \
    ln -s /miniconda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /miniconda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate base" >> ~/.bashrc


# make sure to use miniconda/bin/pip for the pip packages!
RUN /bin/bash -c "source /miniconda/etc/profile.d/conda.sh && \
    conda install -n base python=3.8 -y && \
    conda install -c conda-forge -c bioconda mafft=7.505 fasttree=2.1.11 biopython=1.81 -y && \
    /miniconda/bin/pip install numpy==1.22.3 simplejson==3.17.6 click==8.1.3 pandas==2.0.1 phylo-treetime==0.6.4 ujson==5.2.0 ete3==3.1.3 scipy==1.10.1 pandas==2.0.1 && \
    conda clean -afy && \
    rm -rf /root/.cache"


# copy the entrypoint script
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# binaries to PATH
ENV PATH="/miniconda/bin:/miniconda/condabin:/miniconda/envs/base/bin:$PATH"

ENTRYPOINT ["/entrypoint.sh"]
CMD ["bash"]

