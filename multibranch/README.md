## Create a Multibranch Pipeline job

- `New Item` -> Enter job name -> `Multibranch Pipeline` -> `Ok`

### Setup Project Repository

![image](../doc/Screenshot%202023-07-28%20212800.png)

### Multibranch Scan Webhook Trigger

- Install `Multibranch Scan Webhook Trigger` plugin.

- In the job select `Scan Multibranch Pipeline Triggers` -> `Scan by webhook` and enter your Trigger token

![image](../doc/Screenshot%202023-07-28%20213236.png)

- GitLab -> `Settings` -> `Webhocks` -> Enter URL = `JENKINS_URL/multibranch-webhook-trigger/invoke?token=[Trigger token]`

![image](../doc/Screenshot%202023-07-28%20213439.png)

### Orphaned Item Strategy

- Set `Days to keep old items` and `Max # of old items to keep`

![image](../doc/Screenshot%202023-07-28%20213857.png)
