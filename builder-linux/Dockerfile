ARG base_image
FROM ${base_image}

RUN yum -y update && \
    yum -y install gcc gcc-c++ make patch git && \
    yum -y install bzip2-devel openssl-devel readline-devel && \
    yum clean all

# Install pyenv.
RUN git clone https://github.com/pyenv/pyenv.git /opt/pyenv
ENV PYENV_ROOT=/opt/pyenv
ENV PATH ${PYENV_ROOT}/shims:${PYENV_ROOT}/bin:${PATH}

# Install Python.
ARG python_versions
RUN for VERSION in ${python_versions}; do \
      echo "Installing Python ${VERSION}..." && \
      pyenv install ${VERSION}; \
    done;

# Install Python libraries.
ARG cython_version
RUN for VERSION in ${python_versions}; do \
      echo "Installing libraries on Python ${VERSION}..." && \
      pyenv global ${VERSION} && \
      pip install -U pip setuptools && \
      pip install argparse && \
      pip install Cython==${cython_version} wheel auditwheel; \
    done && \
    pyenv global system

# Install requirements for agents.
RUN yum -y install python-setuptools && yum clean all
RUN easy_install argparse

COPY agent.py /
ENTRYPOINT ["/agent.py"]
