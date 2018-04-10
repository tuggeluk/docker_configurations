FROM nvidia/cuda:8.0-cudnn6-devel
RUN apt-get update && apt-get -y upgrade
# Get required packages
RUN apt-get update && \
  apt-get install vim \
                  python-pip \
                  python-dev \                  
		  python-opencv \
                  python-tk \
                  git \
                  tmux \
                  -y
RUN pip install easydict pandas matplotlib cython
RUN pip install tensorflow-gpu==1.4.1 scipy pillow
RUN git clone https://github.com/cocodataset/cocoapi.git
WORKDIR /cocoapi/PythonAPI 
RUN make install 
