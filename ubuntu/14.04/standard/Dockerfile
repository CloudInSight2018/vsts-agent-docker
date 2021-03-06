FROM microsoft/vsts-agent:ubuntu-14.04

# Install basic command-line utilities
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    curl \
    dnsutils \
    file \
    ftp \
    iproute2 \
    iputils-ping \
    openssh-client \
    sudo \
    telnet \
    time \
    unzip \
    wget \
    zip \
    tzdata \
 && rm -rf /var/lib/apt/lists/*

# Install essential build tools
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    build-essential \
 && rm -rf /var/lib/apt/lists/*

# Install Azure CLI (instructions taken from https://docs.microsoft.com/en-us/cli/azure/install-azure-cli)
RUN echo "deb [arch=amd64] https://packages.microsoft.com/repos/azure-cli/ wheezy main" | tee /etc/apt/sources.list.d/azure-cli.list \
 && apt-key adv --keyserver packages.microsoft.com --recv-keys 52E16F86FEE04B979B07E28DB02C46DF417A0893 \
 && apt-get update \
 && apt-get install -y --no-install-recommends \
    apt-transport-https \
    azure-cli \
 && rm -rf /var/lib/apt/lists/*

# Install CMake
RUN curl -sL https://cmake.org/files/v3.10/cmake-3.10.2-Linux-x86_64.sh -o cmake.sh \
 && chmod +x cmake.sh \
 && ./cmake.sh --prefix=/usr/local --exclude-subdir \
 && rm cmake.sh

# Install Go
RUN curl -sL https://dl.google.com/go/go1.9.4.linux-amd64.tar.gz -o go1.9.4.linux-amd64.tar.gz \
 && mkdir -p /usr/local/go1.9.4 \
 && tar -C /usr/local/go1.9.4 -xzf go1.9.4.linux-amd64.tar.gz --strip-components=1 go \
 && rm go1.9.4.linux-amd64.tar.gz
RUN curl -sL https://dl.google.com/go/go1.10.linux-amd64.tar.gz -o go1.10.linux-amd64.tar.gz \
 && mkdir -p /usr/local/go1.10 \
 && tar -C /usr/local/go1.10 -xzf go1.10.linux-amd64.tar.gz --strip-components=1 go \
 && rm go1.10.linux-amd64.tar.gz
ENV GOROOT_1_9_X64=/usr/local/go1.9.4 \
    GOROOT_1_10_X64=/usr/local/go1.10 \
    GOROOT=/usr/local/go1.10
ENV PATH $PATH:$GOROOT/bin

# Install HHVM
RUN apt-get update \
 && apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xB4112585D386EB94 \
 && add-apt-repository https://dl.hhvm.com/ubuntu \
 && apt-get update \
 && apt-get install -y hhvm \
 && rm -rf /var/lib/apt/lists/*

# Install OpenJDKs
RUN apt-add-repository -y ppa:openjdk-r/ppa
RUN apt-get update \
 && apt-get install -y --no-install-recommends openjdk-7-jdk \
 && rm -rf /var/lib/apt/lists/*
RUN apt-get update \
 && apt-get install -y --no-install-recommends openjdk-8-jdk \
 && rm -rf /var/lib/apt/lists/*
RUN apt-get update \
 && apt-get -o Dpkg::Options::="--force-overwrite" install -y --no-install-recommends openjdk-8-jdk \
 && rm -rf /var/lib/apt/lists/*
RUN update-alternatives --set java /usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java
ENV JAVA_HOME_7_X64=/usr/lib/jvm/java-7-openjdk-amd64 \
    JAVA_HOME_8_X64=/usr/lib/jvm/java-8-openjdk-amd64 \
    JAVA_HOME_8_X64=/usr/lib/jvm/java-8-openjdk-amd64 \
    JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64 \
    JAVA_TOOL_OPTIONS=-Dfile.encoding=UTF8

# Install Java Tools (Ant, Gradle, Maven)
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    ant \
    ant-optional \
 && rm -rf /var/lib/apt/lists/*
RUN curl -sL https://services.gradle.org/distributions/gradle-4.6-bin.zip -o gradle-4.6.zip \
 && unzip -d /usr/share gradle-4.6.zip \
 && ln -s /usr/share/gradle-4.6/bin/gradle /usr/bin/gradle \
 && rm gradle-4.6.zip
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    maven \
 && rm -rf /var/lib/apt/lists/*
ENV ANT_HOME=/usr/share/ant \
    GRADLE_HOME=/usr/share/gradle \
    M2_HOME=/usr/share/maven

# Install MySQL Client
RUN apt-get update \
  && apt-get install mysql-client -y \
  && rm -rf /var/lib/apt/lists/*
ENV mysql=/usr/bin/mysql

# Install .NET Core SDK and initialize package cache
RUN curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > /etc/apt/trusted.gpg.d/microsoft.gpg \
 && echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-trusty-prod trusty main" > /etc/apt/sources.list.d/dotnetdev.list \
 && apt-get update \
 && apt-get install -y --no-install-recommends \
    apt-transport-https \
    dotnet-sdk-2.1.101 \
 && rm -rf /var/lib/apt/lists/*
RUN dotnet help
ENV dotnet=/usr/bin/dotnet

# Install LTS Node.js and related build tools
RUN curl -sL https://git.io/n-install | bash -s -- -ny - \
 && ~/n/bin/n lts \
 && npm install -g bower grunt gulp n \
 && rm -rf ~/n
ENV bower=/usr/local/bin/bower \
    grunt=/usr/local/bin/grunt

# Install Powershell Core
RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add - \
 && curl https://packages.microsoft.com/config/ubuntu/14.04/prod.list | tee /etc/apt/sources.list.d/microsoft.list \
 && apt-get update \
 && apt-get install -y --no-install-recommends \
    powershell \
 && rm -rf /var/lib/apt/lists/*

# Install Python
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    python \
    python-pip \
    python3 \
    python3-pip \
 && rm -rf /var/lib/apt/lists/*

# Install Scala build tools
RUN curl -s https://raw.githubusercontent.com/paulp/sbt-extras/master/sbt > /usr/local/bin/sbt \
 && chmod 0755 /usr/local/bin/sbt

# Install Subversion
RUN apt-get update \
 && apt-get install -y --no-install-recommends \
    subversion \
 && rm -rf /var/lib/apt/lists/*
