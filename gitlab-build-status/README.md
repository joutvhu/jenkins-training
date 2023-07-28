## Update GitLab Build Status

### Add GitLab connection

- Create a GitLab Access Token with api scope on GitLab, then add token to credentials

- Add GitLab Connection

![image](../doc/Screenshot%202023-07-29%20050412.png)

### Config Jenkinsfile

- Add gitLabConnection and gitlabBuilds

```
options {
    gitLabConnection gitLabConnection: 'Gitlab', jobCredentialId: 'Gitlab_Token', useAlternativeCredential: true
    gitlabBuilds(builds: ['Build Project', 'Junit Reports', 'SonarQube Analysis', 'Deploy to Nexus'])
}
```

- Now you will have status like this

![image](../doc/Screenshot%202023-07-29%20050308.png)
