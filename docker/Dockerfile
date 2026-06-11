FROM ubuntu:26.04

# Avoid interactive prompts during package installation
ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
  apt-get install -y --no-install-recommends \
    libglib-perl \
    libglib-object-introspection-perl \
    libipc-run-perl \
    libjson-perl \
    libset-scalar-perl \
    libxml-libxml-perl \
    libxml-libxslt-perl \
    gir1.2-appstream \
    openjdk-17-jdk \
    build-essential \
    glslc \
    gobject-introspection \
    libglib2.0-dev-bin \
    libxml2-utils \
    meson \
    ninja-build \
    sassc \
    git \
    curl \
    less \
    nano \
    unzip \
    wget && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*

RUN cd ~ && git clone https://github.com/sp1ritCS/gtk-android-builder.git \
  && cd gtk-android-builder \
  && make install

RUN mkdir -p ~/Android/Sdk/cmdline-tools

RUN cd ~/Android/Sdk/cmdline-tools \
  && wget https://dl.google.com/android/repository/commandlinetools-linux-14742923_latest.zip \
  && unzip commandlinetools-linux-14742923_latest.zip \
  && mv cmdline-tools latest

RUN echo "export ANDROID_HOME=$HOME/Android/Sdk \
  export ANDROID_SDK_ROOT=$ANDROID_HOME \
  export PATH=$PATH:$ANDROID_HOME/cmdline-tools/latest/bin \
  export PATH=$PATH:$ANDROID_HOME/platform-tools \
  export PATH=$PATH:$ANDROID_HOME/build-tools \
" >> ~/.bashrc

RUN . ~/.bashrc && cd $ANDROID_HOME/cmdline-tools/latest/bin \
  && yes | ./sdkmanager --licenses \
  && ./sdkmanager --update \
  && ./sdkmanager \
    "platform-tools" \
    "build-tools;36.0.0" \
    "platforms;android-36" \
    "cmake;3.22.1" \
  && ./sdkmanager "ndk;29.0.14206865"

RUN mkdir -p ~/dlang && wget https://dlang.org/install.sh -O ~/dlang/install.sh && chmod 755 ~/dlang/install.sh

RUN ~/dlang/install.sh ldc
RUN ~/dlang/install.sh dub

RUN echo ". ~/dlang/ldc-1.42.0/activate" >> ~/.bashrc

COPY meson.patch /root
RUN cd / && patch -p0 </root/meson.patch
