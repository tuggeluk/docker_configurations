FROM nvidia/cuda:8.0-cudnn6-devel
WORKDIR /root

RUN apt-get update && apt-get -y upgrade
# Get required packages
RUN apt-get update && \
  apt-get install vim \
                  python-pip \
                  python-dev \
                  python-opencv \
                  python-tk \
                  libjpeg-dev \
                  libfreetype6 \
                  libfreetype6-dev \
                  zlib1g-dev \
                  cmake \
                  wget \
                  cython \
                  git \
		  tmux \
                  -y

# Get required python modules, with downgraded tensorflow
RUN pip install --upgrade pip
RUN pip install -U image \
		django==1.11.8 \               
		scipy \
                matplotlib \
                PyYAML \
                numpy \
                easydict \   
                jupyter \
                tensorflow-gpu==1.4.1 \
                pillow \
		scikit-image



# Add CUDA to the path
ENV LD_LIBRARY_PATH $LD_LIBRARY_PATH:/usr/local/cuda/lib64
ENV CUDA_HOME /usr/local/cuda  
ENV PYTHONPATH /root/coco/PythonAPI
RUN ldconfig
    
    
# Set up our notebook config.
COPY jupyter_notebook_config.py /root/.jupyter/

COPY tmux.conf /root/.tmux.conf

# TensorBoard
EXPOSE 6006
# IPython
EXPOSE 8888


# install sshd and expose it (from "https://github.com/rastasheep")

RUN apt-get update

RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd

RUN echo 'root:root' |chpasswd

RUN sed -ri 's/^PermitRootLogin\s+.*/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config
RUN echo "AllowTcpForwarding yes" >>  /etc/ssh/sshd_config

EXPOSE 22
#CMD /bin/bash

# install vnc (from dorowu/ubuntu-desktop-lxde-vnc)
RUN sed -i 's#http://archive.ubuntu.com/#http://tw.archive.ubuntu.com/#' /etc/apt/sources.list

# built-in packages
RUN apt-get update \
    && apt-get install -y --no-install-recommends software-properties-common curl \
    && sh -c "echo 'deb http://download.opensuse.org/repositories/home:/Horst3180/xUbuntu_16.04/ /' >> /etc/apt/sources.list.d/arc-theme.list" \
    && curl -SL http://download.opensuse.org/repositories/home:Horst3180/xUbuntu_16.04/Release.key | apt-key add - \
    && add-apt-repository ppa:fcwu-tw/ppa \
    && apt-get update \
    && apt-get install -y --no-install-recommends --allow-unauthenticated \
        supervisor \
        openssh-server pwgen sudo vim-tiny \
        net-tools \
        lxde x11vnc xvfb \
        gtk2-engines-murrine ttf-ubuntu-font-family \
        libreoffice firefox \
        fonts-wqy-microhei \
        language-pack-zh-hant language-pack-gnome-zh-hant firefox-locale-zh-hant libreoffice-l10n-zh-tw \
        nginx \
        python-pip python-dev build-essential \
        mesa-utils libgl1-mesa-dri \
        gnome-themes-standard gtk2-engines-pixbuf gtk2-engines-murrine pinta arc-theme \
        dbus-x11 x11-utils \
    && apt-get autoclean \
    && apt-get autoremove \
    && rm -rf /var/lib/apt/lists/*


# tini for subreap                                   
ENV TINI_VERSION v0.9.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /bin/tini
RUN chmod +x /bin/tini

ADD image /
RUN pip install setuptools wheel && pip install -r /usr/lib/web/requirements.txt

EXPOSE 80
# install svg-packages
run pip install --upgrade cffi
run pip install svgpathtools
run pip install cairosvg==1.0.22
run pip install pandas

# install py-coco tools
WORKDIR /
RUN git clone https://github.com/cocodataset/cocoapi.git
WORKDIR /cocoapi/PythonAPI
RUN make install

WORKDIR /root
ENV HOME=/home/ubuntu \
    SHELL=/bin/bash
CMD  ["/bin/bash"]



#CMD    ["/usr/sbin/sshd", "-D"]
