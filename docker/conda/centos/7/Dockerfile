FROM wsinpg/centos-7-base:latest as installer

ENV CONDA_GROUP_ID=32001 \
    LANG=en_GB.UTF-8 \
    LANGUAGE=en_GB \
    LC_ALL=en_GB.UTF-8

COPY . /opt/docker/conda

RUN yum -y update && \
    yum -y install \
    bzip2 \
    glibc-langpack-en && \
    localedef -i en_GB -c -f UTF-8 \
    -A /usr/share/locale/locale.alias en_GB.UTF-8 && \
    /opt/docker/conda/scripts/install_conda.sh && \
    cp /root/.condarc /opt/conda/etc/condarc && \
    chmod +x /opt/docker/conda/scripts/docker-entrypoint.sh && \
    groupadd --gid $CONDA_GROUP_ID conda && \
    chgrp -R conda /opt/conda && \
    chmod -R g=u /opt/conda

FROM wsinpg/centos-7-base:latest

ENV CONDA_GROUP_ID=32001 \
    LANG=en_GB.UTF-8 \
    LANGUAGE=en_GB \
    LC_ALL=en_GB.UTF-8

RUN yum -y update && \
    yum install -y \
    glibc-langpack-en \
    rsync \
    vim && \
    localedef -i en_GB -c -f UTF-8 \
    -A /usr/share/locale/locale.alias en_GB.UTF-8 && \
    yum autoremove -y && \
    yum -y clean all && \
    groupadd -g $CONDA_GROUP_ID conda

COPY --from=installer --chown=0:conda /opt/conda /opt/conda
COPY --from=installer /opt/docker/conda/scripts/docker-entrypoint.sh \
    /opt/docker/conda/scripts/docker-entrypoint.sh

ENTRYPOINT ["/opt/docker/conda/scripts/docker-entrypoint.sh"]

CMD ["/bin/bash"]
