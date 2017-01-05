
#Dockerfile
#
# Version:          1
# Software:         GATK
# Software Version: 3.4
# Description:      The Genome Analysis Toolkit or GATK is a software package developed at the Broad Institute to analyze high-throughput sequencing data.
# Website:          https://www.broadinstitute.org/gatk/|https://github.com/broadgsa/gatk-protected/
# Tags:             Genomics
# Provides:         gatk 3.4
# Base Image:       java:7

# Source Image
FROM java:7

# Set noninterative mode
ENV DEBIAN_FRONTEND noninteractive

USER root

# INSTALL MAVEN
#
RUN apt-get update -qq && \
    apt-get install -y -q --fix-missing \
      libncurses5-dev  \
      libncursesw5-dev \
      build-essential \
      bzip2 \
      zlib1g-dev \
      maven && \
    apt-get clean && \
    apt-get purge && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* && \
    mkdir /data /config


#
# Create $HOME/bin folder
#
RUN mkdir -p /home/biodocker/bin



# Add $HOME/bin to path

ENV PATH=$PATH:/home/biodocker/bin
ENV HOME=/home/biodocker


#
# INSTALL GATK
#
# download and build GATK from current master
ENV SHA=f19618653a0d23baaf147efe7f14aeb4eeb0cbb8

ENV DST=/home/biodocker/bin
ENV FOLDER=gatk

RUN git clone https://github.com/broadgsa/gatk-protected.git $DST/$FOLDER && \
    cd $DST/$FOLDER && \
    git reset --hard $SHA && \
    mvn verify && \
    bash -c 'echo -e "#!/bin/bash\njava -jar '$DST/$FOLDER'/target/GenomeAnalysisTK.jar  \$@" > '$DST'/GenomeAnalysisTK' && \
    chmod +x $DST/GenomeAnalysisTK

ENV PATH=/usr/lib/jvm/java-7-openjdk-amd64/bin:$PATH

#ENV JAVABIN=/usr/lib/jvm/java-7-openjdk-amd64/bin/java
#ENTRYPOINT ["java", "-jar", "$DST/target/GenomeAnalysisTK.jar"]

#
# Share default volumes
#
VOLUME ["/data", "/config"]

CMD ["GenomeAnalysisTK"]

##################### INSTALLATION END #####################
