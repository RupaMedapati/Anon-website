Implementing CI/CD Pipeline using Declarative | github|maven|jenkins|sonarqube|tomcat|aws s3
jenkins installation:
ubuntu->t2 small->launch instance
apt-get update -y
apt-get install fontconfig openjdk-11-jre
java -version
update-alternatives --config java->it will show java installed path
JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64->setup variable path
PATH=$PATH:$JAVA_HOME->adding variable to the path.
export JAVA_HOME PATH
java -version
download jenkins :
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee\ /usr/share/keyrings/jenkins-keyring.asc > /dev/null
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
    https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null
    sudo apt-get update -y
    sudo apt-get install jenkins -y
    service jenkins status
    install maven for that go to cd /opt
    mkdir maven
    cd maven/
    wget https://dlcdn.apache.org/maven/maven-3/3.9.6/binaries/apache-maven-3.9.6-bin.tar.gz
    tar -xvzf apache-maven-3.9.6-bin.tar.gz
    mv apache-maven-3.9.6 maven->renaming the package name
    M2_HOME=/opt/maven/maven
    M2=/opt/maven/maven/bin
    PATH=$PATH:$JAVA_HOME:$M2_HOME:$M2
    export M2_HOME M2 PATH
    mvn --version




