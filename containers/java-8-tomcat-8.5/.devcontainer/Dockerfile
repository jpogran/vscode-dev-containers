#-------------------------------------------------------------------------------------------------------------
# Copyright (c) Microsoft Corporation. All rights reserved.
# Licensed under the MIT License. See https://go.microsoft.com/fwlink/?linkid=2090316 for license information.
#-------------------------------------------------------------------------------------------------------------

FROM tomcat:8.5-jre8

# Avoid warnings by switching to noninteractive
ENV DEBIAN_FRONTEND=noninteractive
# Set the default shell to bash rather than sh
ENV SHELL /bin/bash
# Update JAVA_HOME
ENV JAVA_HOME /docker-java-home
# Maven install settings
ARG MAVEN_VERSION=3.6.1
ARG MAVEN_SHA=b4880fb7a3d81edd190a029440cdf17f308621af68475a4fe976296e71ff4a4b546dd6d8a58aaafba334d309cc11e638c52808a4b0e818fc0fd544226d952544
ENV MAVEN_HOME /usr/share/maven
ENV MAVEN_CONFIG /root/.m2

# Configure apt and install packages
RUN apt-get update \
    && apt-get -y install --no-install-recommends apt-utils 2>&1 \
    #
    # Verify git, needed tools installed
    && apt-get -y install git procps curl lsb-release \
    #
    # Install openjdk 8
    && apt-get -y install --no-install-recommends openjdk-8-jdk \
    #
    # Install Maven tools
    && mkdir -p /usr/share/maven /usr/share/maven/ref \
    && curl -fsSL -o /tmp/apache-maven.tar.gz https://apache.osuosl.org/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
    && echo "${MAVEN_SHA} /tmp/apache-maven.tar.gz" | sha512sum -c - \
    && tar -xzf /tmp/apache-maven.tar.gz -C /usr/share/maven --strip-components=1 \
    && rm -f /tmp/apache-maven.tar.gz \
    && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn \
    #
    # Clean up
    && apt-get autoremove -y \
    && apt-get clean -y \
    && rm -rf /var/lib/apt/lists/*

# Copy maven-settings.xml
COPY maven-settings.xml /usr/share/maven/ref/

# Switch back to dialog for any ad-hoc use of apt-get
ENV DEBIAN_FRONTEND=dialog