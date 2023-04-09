FROM maven:3-eclipse-temurin-8
RUN <<__EOS__
    apt update
    apt install -y wget apt-transport-https
    mkdir -p /etc/apt/keyrings
    wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | tee /etc/apt/keyrings/adoptium.asc
    echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb focal main" | tee /etc/apt/sources.list.d/adoptium.list
    apt update
    apt install -y temurin-17-jdk
    git clone https://github.com/redhat-developer/rsp-server-community
    cd rsp-server-community/rsp
    mvn clean install
__EOS__
