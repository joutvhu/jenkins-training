# Jenkins Training

Run the following to create containers.

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

- Install tools (JDK, Maven): `Manage Jenkins` -> `Tools``: Select `Install automatically``

- Create SSH key. Open terminal in container and create a SSH key.

### GitLab Setting

- Get SSH public key from Jenkins and add to Gitlab.

- GitLab settings Admin -> `Settings` -> `Network` -> `Outbound Requests` -> `Allow requests to the local network from hooks and services.`

### Create Gitlab Project

- Create a repository with name `demo`

- Open the repository -> `Settings` -> `Integrations` -> `Jenkins`

  - Jenkins server URL: `http://jenkins.jenkins:8080`

  - Enable SSL verification: disable.

  - Select events.

- If you use Github then use `Webhook` instead of `Integration` (`Settings` -> `Webhooks` -> `Add webhook`)

### Create Jenkins Pipeline Project

- Create a Pipeline project with name `demo`

- Enable _"Build when a change is pushed to GitLab. GitLab webhook URL: http://localhost:58080/project/demo"_

  - Select events.

- Add repository link, example `git@gitlab.jenkins:root/demo.git`
