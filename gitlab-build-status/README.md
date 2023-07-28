## Update GitLab Build Status

### Add GitLab connection

- Create a GitLab Access Token with api scope on GitLab, then add token to credentials

- Add GitLab Connection

![image](../doc/Screenshot%202023-07-29%20050412.png)

### Config Jenkinsfile

- Add gitLabConnection and gitlabCommitStatus

```
options {
    gitLabConnection gitLabConnection: 'Gitlab', jobCredentialId: 'Gitlab_Token', useAlternativeCredential: true
    gitlabCommitStatus('Build with Jenkins')
}
```

- Now you will have status like this

![image](../doc/Screenshot%202023-07-29%20050308.png)
