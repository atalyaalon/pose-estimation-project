FROM nvidia/cuda

# Install some basic utilities
RUN apt-get update && apt-get install -y \
    curl \
    ca-certificates \
    sudo \
    git \
    bzip2 \
    libx11-6 \
    unzip \
    wget \
    time \
    python3-pip python3-dev \
  && cd /usr/local/bin \
  && ln -s /usr/bin/python3 python \
  && pip3 install --upgrade pip

# get data
  RUN mkdir data-mscoco && cd data-mscoco && \
  wget http://images.cocodataset.org/annotations/annotations_trainval2017.zip && \
  unzip annotations_trainval2017.zip && \
  wget http://images.cocodataset.org/annotations/image_info_test2017.zip && \
  unzip image_info_test2017.zip && \
  mkdir images && cd images && \
  wget http://images.cocodataset.org/zips/val2017.zip && \
  unzip val2017.zip && \
  wget http://images.cocodataset.org/zips/train2017.zip && \
  unzip train2017.zip

# make output dir
RUN mkdir outputs

# First copy only the requirement.txt, so changes in other files won't trigger
# a full pip reinstall


RUN pip3 install torch==1.4.0 torchvision==0.4.0 openpifpaf==0.9

CMD python -m openpifpaf.train --lr=1e-3  --momentum=0.95  --epochs=7  --lr-decay 120 140   --batch-size=16 --basenet=resnet101   --head-quad=1 --headnets pif paf paf25   --square-edge=401   --lambdas 10 1 1 15 1 1 15 1 1 --loader-workers=16 --n-images=32
