# Jenkins Training

Run the following commands to create containers.

```
docker network create jenkins

docker run --name jenkins -p 50000:50000 -p 58080:8080 --network jenkins jenkins/jenkins

docker run --name gitlab -p 5022:22 -p 5443:443 -p 5080:80 --network jenkins gitlab/gitlab-ce
```

### Jenkins Admin Password

- Find in file `/var/jenkins_home/secrets/initialAdminPassword`

### Gitlab Admin User

- Username: `root`
- Password in file: `/etc/gitlab/initial_root_password`

### Jenkins Setting

- Install Gitlab plugin

- Install tools (JDK, Maven): `Manage Jenkins` -> `Tools` : Select `Install automatically`

![image](doc/Screenshot%202023-07-16%20010405.png)

![image](doc/Screenshot%202023-07-16%20010525.png)

- Create SSH key. Open terminal in container and create a SSH key.

![image](doc/Screenshot%202023-07-16%20011323.png)

### GitLab Setting

- Get SSH public key from Jenkins and add to Gitlab.

- GitLab settings Admin -> `Settings` -> `Network` -> `Outbound Requests` -> `Allow requests to the local network from hooks and services.`

### Create Gitlab Project

- Create a repository with name `demo`

- Open the repository -> `Settings` -> `Integrations` -> `Jenkins`

  - Jenkins server URL: `http://jenkins.jenkins:8080`

  - Enable SSL verification: disable.

  - Select events.

![image](doc/Screenshot%202023-07-16%20010734.png)

- If you use Github then use `Webhook` instead of `Integration` (`Settings` -> `Webhooks` -> `Add webhook`)

### Create Jenkins Pipeline Project

- Create a Pipeline project with name `demo`

- Enable _"Build when a change is pushed to GitLab. GitLab webhook URL: http://localhost:58080/project/demo"_

  - Select events.

![image](doc/Screenshot%202023-07-16%20010615.png)

- Add repository link, example `git@gitlab.jenkins:root/demo.git`

![image](doc/Screenshot%202023-07-16%20010649.png)

## Setup SonarQube

```
docker run --name sonarqube -p 59000:9000 sonarqube

docker network connect jenkins sonarqube
```

- Default account: `admin`/`admin`

### Connect SonarQube to Gitlab

- Gitlab -> User Settings -> Access Tokens

![image](doc/Screenshot%202023-07-19%20205711.png)

- Connect SonarQube with Gitlab

![image](doc/Screenshot%202023-07-19%20205950.png)


### Install Plugin

- Install SonarQube Scanner pluggin into Jenkins.

- Create Token in SonarQube: User -> My Account -> Security -> Generate Token

![image](doc/Screenshot%202023-07-19%20201758.png)

- Manage Jenkins -> System -> Add SonarQube

![image](doc/Screenshot%202023-07-19%20201923.png)

- Connect 

![image](doc/Screenshot%202023-07-19%20202119.png)

### Config Maven Project

- Add the following dependency

```
<dependency>
  <groupId>org.sonarsource.scanner.maven</groupId>
  <artifactId>sonar-maven-plugin</artifactId>
  <version>3.9.1.2184</version>
</dependency>
```

- Add the following step into Jenkinsfile

```
stage('SonarQube Analysis') {
    steps {
        withSonarQubeEnv('SonarQube') {
            sh '''
                mvn clean verify sonar:sonar -Dsonar.projectName='Demo' -Dsonar.branch.name=${BRANCH_NAME}
               '''
        }
    }
}
```