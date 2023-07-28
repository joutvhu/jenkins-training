## Update GitLab Build Status

### Add GitLab connection

- Create a GitLab Access Token with api scope on GitLab, then add token to credentials

- Add GitLab Connection

![image](../doc/Screenshot%202023-07-29%20050412.png)

### Config Jenkinsfile

- Add gitLabConnection

```
options {
    gitLabConnection gitLabConnection: 'Gitlab', jobCredentialId: 'Gitlab_Token', useAlternativeCredential: true
}
```

- Add first stage to notify to GitLab that job is running

```
stage('Notify GitLab') {
    steps {
        updateGitlabCommitStatus name: 'build', state: 'running'
    }
}
```

- Add `post` block to notify build status to GitLab

```
post {
    aborted {
        updateGitlabCommitStatus name: 'build', state: 'canceled'
    }
    failure {
        updateGitlabCommitStatus name: 'build', state: 'failed'
    }
    success {
        updateGitlabCommitStatus name: 'build', state: 'success'
    }
}
```

- Now you will have status like this

![image](../doc/Screenshot%202023-07-29%20050308.png)
