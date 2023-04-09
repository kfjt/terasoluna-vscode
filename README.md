# terasoluna-vscode
Terasoluna official installs Java and STS in the local environment, but I prefer VSCode and DevContainer.

see
- http://terasolunaorg.github.io/guideline/current/ja/Overview/FirstApplication.html

and
- https://code.visualstudio.com/docs/java/java-tutorial
- https://code.visualstudio.com/docs/java/java-tomcat-jetty

## Clone and Execute
- Required
    - git
    - vscode
    - docker
1. git clone https://github.com/kfjt/terasoluna-vscode.git
1. code terasoluna-vscode
1. Open Folder in Container
1. Open Folder > helloworld
1. MAVEN > TERASOLUNA Server Framework...
    - Run Maven Commands ...
    - install
1. SERVERS > Community Server Connector
    - Create New Server
        - tomcat 9.*
    - Edit Server
        - `"vm.install.path": "/opt/java/openjdk/"`
    - Add Deployment > File
        - /workspaces/terasoluna-vscode/helloworld/target/helloworld.war
    - Start Server
    - Publish Server (Full)
    - Server Actions...
        - Show in browser
        - http://localhost:8080/helloworld

## Repository creation and operation check procedure
1. .gitignore
    ```sh
    curl -o .gitignore https://www.toptal.com/developers/gitignore/api/git,java,visualstudiocode,maven
    ```

1. .devcontainer
    ```sh
    mkdir .devcontainer
    >.devcontainer/Dockerfile cat - << __EOF__
    FROM maven:3-eclipse-temurin-8
    RUN <<__EOS__
        apt update
        apt install -y wget apt-transport-https
        mkdir -p /etc/apt/keyrings
        wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | tee /etc/apt/keyrings/adoptium.asc
        echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print$2}' /etc/os-release) main" | tee /etc/apt/sources.list.d/adoptium.list
        apt update
        apt install -y temurin-17-jdk
        git clone https://github.com/redhat-developer/rsp-server-community
        cd rsp-server-community/rsp
        mvn clean install
    __EOS__
    __EOF__
    >.devcontainer/devcontainer.json cat - << __EOF__
    {
        "name": "terasoluna-vscode",
        "build": {
            "dockerfile": "Dockerfile"
        },
        "customizations": {
            "vscode": {
                "extensions": [
                    "vscjava.vscode-java-pack",
                    "redhat.vscode-community-server-connector"
                ]
            }
        }
    }
    __EOF__
    ```

1. Open Folder in Container

1. mvn archetype:generate

    see: http://terasolunaorg.github.io/guideline/current/ja/Overview/FirstApplication.html#id3
    ```sh
    mvn archetype:generate -B \
    -DarchetypeGroupId=org.terasoluna.gfw.blank \
    -DarchetypeArtifactId=terasoluna-gfw-web-blank-archetype \
    -DarchetypeVersion=5.7.2.RELEASE \
    -DgroupId=com.example.helloworld \
    -DartifactId=helloworld \
    -Dversion=1.0.0-SNAPSHOT
    ```

1. helloworld/.vscode
    ```sh
    mkdir helloworld/.vscode
    >helloworld/.vscode/extensions.json cat - << __EOF__
    {
        "recommendations": [
            "ms-vscode-remote.vscode-remote-extensionpack",
            "mhutchie.git-graph"
        ]
    }
    __EOF__
    >helloworld/.vscode/settings.json cat - << __EOF__
    {
        "rsp-ui.enableStartServerOnActivation": [
            {
                "id": "redhat.vscode-community-server-connector",
                "name": "Community Server Connector",
                "startOnActivation": true
            }
        ],
        "rsp-ui.rsp.java.home": "/usr/lib/jvm/temurin-17-jdk-amd64/",
        "redhat.telemetry.enabled": false
    }
    __EOF__
    ```

1. Open Folder > helloworld

1. MAVEN > TERASOLUNA Server Framework...
    - Run Maven Commands ...
    - install

1. SERVERS > Community Server Connector
    - Create New Server
        - tomcat 9.*
    - Edit Server
        - `"vm.install.path": "/opt/java/openjdk/"`
    - Add Deployment > File
        - /workspaces/terasoluna-vscode/helloworld/target/helloworld.war
    - Start Server
    - Publish Server (Full)
    - Server Actions...
        - Show in browser
        - http://localhost:8080/helloworld
