
## Setup SonarQube

```
docker run --name sonarqube -p 59000:9000 sonarqube

docker network connect jenkins sonarqube
```

- Default account: `admin`/`admin`

### Connect SonarQube to Gitlab

- Gitlab -> `User Settings` -> `Access Tokens`

![image](../doc/Screenshot%202023-07-19%20205711.png)

- Connect SonarQube with Gitlab

![image](../doc/Screenshot%202023-07-19%20205950.png)


### Install Plugin

- Install SonarQube Scanner pluggin into Jenkins.

- Create Token in SonarQube: `User` -> `My Account` -> `Security` -> Generate Token

![image](../doc/Screenshot%202023-07-19%20201758.png)

- Manage Jenkins -> System -> Add SonarQube

![image](../doc/Screenshot%202023-07-19%20201923.png)

- Connect 

![image](../doc/Screenshot%202023-07-19%20202119.png)

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