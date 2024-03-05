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
    open jenkins in browser-> manage jenkins->tools->jdk ->add jdk->name(JAVA_HOME) and add path also-> go down slowly and click on maven ->name(M2_HOME) /opt/maven/maven--->save
    mvn --version
    install git->apt-get install git -y
    git --version
    in jenkins>new item->pipeline project
    syntax: 
    pipeline{
         agent any
         tools{
         maven 'M2_HOME'
         jdk 'JAVA_HOME'
         }
         stages {
             stage('checking out code from github'){
                 steps{
                     git branch: 'main',
                     url: ''
             }
         }
         stage('perform unit testing'){
                 steps{
                     sh 'mvn clean test'
             }
         }
    
         stage('compile the code'){
                 steps{
                     sh 'mvn clean install'
             }
         }
         stage('sonaqube analysis'){
                 steps{
                     script{
                         withSonarQubeEnv('provide name we used for sonar in manage jenkins system page'){
                         sh 'mvn sonar:sonar'
                         }
                     }
             }
             stage('upload to s3 artifactory'){
                 steps{
                     sh 'aws s3 cp '/var/lib/jenkins/workspace/pipeline_script/webapp/target/webapp.war' s3://s3bucketname/snapshot_artifact/12_war"
             }
         }
         }
    }

sonarqube instance->aws linux->t2.mediem->launch instance
go to root acc->yum update -y
yum install java-17-amazon-corretto.x86_64 -y
to install sonarqube create one directory cd /opt
mkdir sonar
cd sonar/
sonar doesn't support root acc for that create one user useradd sonar, passwd sonar
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-10.4.1.88267.zip
unzip sonarqube-10.4.1.88267.zip
mv  sonarqube-10.4.1.88267 sonar
chown -R sonar:sonar sonar
cd sonar/
su sonar
cd bin/
cd linux-x86-64
sh sonar.sh start
go to browser open sonarqube give admin and passwd credentials
go to jenkins->manage jenkins->plugins->available plugins->sonarqube scanner
add sonarqube with jenkins->manage jenkins->system->sonarqubeservers->add->name(sonar-server),url(give browser url),for authentication go to sonarqube site->myaccount->name and geneate token
now go to IAM->roles->create role->aws service->ec2->s3fullaccess->next->create role.
select jenkins server->modify iam role ->update
create one bucket

deploy artifact into tomcat:
ec2instance->linux->t2micro->launch instance
yum install java-1.8*
java -version
cd /opt
mkdir tomcat
useradd tomcat
passwd tomcat
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.86/bin/apache-tomcat-9.0.86.tar.gz
mv apache-tomcat-9.0.86.tar.gz tomcat
cd tomcat
tar -xvzf apache-tomcat-9.0.86.tar.gz
mv apache-tomcat-9.0.86 tomcat
cd tomcat/
cd ..
chown -R tomcat:tomcat tomcat
cd tomcat
cd bin
sh startup.sh





