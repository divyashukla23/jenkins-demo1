# GitHub Webhook Setup in Jenkins

This guide explains how to configure a GitHub webhook to automatically trigger Jenkins builds whenever code is pushed to a GitHub repository.

---

## Prerequisites

* Jenkins server is installed and running.
* Jenkins is accessible from GitHub (public IP or domain).
* A GitHub repository.
* Administrative access to Jenkins and the GitHub repository.

---

## Step 1: Install Required Plugins

1. Open Jenkins.
2. Navigate to **Manage Jenkins → Plugins**.
3. Install the following plugins:

   * Git Plugin
   * GitHub Plugin
   * GitHub Integration Plugin (Recommended)
4. Restart Jenkins if prompted.

---

## Step 2: Configure the Jenkins Job

### For Freestyle Project

1. Open the Jenkins job.

2. Click **Configure**.

3. Scroll to **Build Triggers**.

4. Enable:

   ```
   GitHub hook trigger for GITScm polling
   ```

5. Save the configuration.

### For Pipeline Project

1. Open the Pipeline job.

2. Click **Configure**.

3. Under **Build Triggers**, enable:

   ```
   GitHub hook trigger for GITScm polling
   ```

4. Save the job.

---

## Step 3: Get the Jenkins Webhook URL

Find your Jenkins URL:

```
Manage Jenkins → System → Jenkins URL
```

Append the following endpoint:

```
/github-webhook/
```

### Example

If your Jenkins URL is:

```
http://54.210.100.10:8080
```

Your webhook URL will be:

```
http://54.210.100.10:8080/github-webhook/
```

If using HTTPS:

```
https://jenkins.company.com/github-webhook/
```

---

## Step 4: Configure the Webhook in GitHub

1. Open your GitHub repository.
2. Go to:

```
Settings → Webhooks
```

3. Click **Add webhook**.

Configure the webhook with the following values:

| Field        | Value                                  |
| ------------ | -------------------------------------- |
| Payload URL  | `http://<jenkins-url>/github-webhook/` |
| Content Type | `application/json`                     |
| Secret       | Optional                               |
| Events       | Just the push event                    |

4. Click **Add webhook**.

---

## Step 5: Test the Webhook

1. Make a commit and push it to the GitHub repository.
2. Jenkins should automatically start a new build.

To verify the webhook delivery:

```
GitHub Repository
→ Settings
→ Webhooks
→ Select your webhook
→ Recent Deliveries
```

A successful delivery should return:

```
HTTP Status: 200 OK
```

---

## Troubleshooting

### Jenkins build is not triggered

* Verify the webhook URL is correct.
* Ensure Jenkins is reachable from GitHub.
* Confirm **GitHub hook trigger for GITScm polling** is enabled.
* Check the Jenkins job configuration.
* Review Jenkins system logs for errors.

### Webhook returns 404

Verify the URL ends with:

```
/github-webhook/
```

### Webhook returns 403

* Check Jenkins security settings.
* Verify authentication and CSRF protection configuration if applicable.

### Jenkins is running on AWS EC2

Ensure the EC2 Security Group allows inbound traffic on:

* Port **8080** (HTTP), or
* Port **443** (HTTPS)

Also ensure Jenkins is accessible via a public IP address or domain name.

---

## Summary

1. Install GitHub-related plugins.
2. Enable **GitHub hook trigger for GITScm polling**.
3. Get the Jenkins URL and append `/github-webhook/`.
4. Add the webhook in GitHub.
5. Push code and verify that Jenkins automatically starts the build.
