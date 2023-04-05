FROM mambaorg/micromamba:1.4-focal

LABEL authors="Nicolas Vannieuwkerke <nicolas.vannieuwkerke@ugent.be>" \
    description="Docker image containing AnnotSV"

ENV VERSION=3.3.2
ENV ANNOTSV=/opt/AnnotSV/${VERSION}
ENV PATH="${PATH}:${ANNOTSV}/bin"


COPY AnnotSV.our /tmp/
USER root
RUN mkdir -p "$ANNOTSV/bin" && chown -R mambauser /opt/AnnotSV && chown mambauser /tmp/AnnotSV.our

USER mambauser

# Install mamba dependencies
RUN micromamba install -y --name base -c conda-forge -c bioconda -c defaults \
    git \
    make \
    wget \
    coreutils \
    tk=8.6.12 \
    bedtools=2.30 \
    bcftools=1.16 \
    openjdk=8 \
    unzip \
    python=3.8 \
    natsort=7.1.1 \
    pandas=1.5.3 \
    pyfaidx=0.7.2.1 \
    && micromamba clean --all --yes

ARG MAMBA_DOCKERFILE_ACTIVATE=1

RUN mkdir -p /tmp/install && cd /tmp/install  && wget https://github.com/lgmgeo/AnnotSV/archive/refs/tags/v$VERSION.tar.gz && \
    tar -xvf v$VERSION.tar.gz -C . && \
    cd AnnotSV* && \
    make PREFIX=${ANNOTSV} install install-exomiser && cd / && rm -rf /tmp/install && \
    mv "$ANNOTSV/bin/AnnotSV" "$ANNOTSV/bin/AnnotSV.real" && \
    mv /tmp/AnnotSV.our "$ANNOTSV/bin/AnnotSV" && \
    chmod a+x "$ANNOTSV/bin/AnnotSV"
