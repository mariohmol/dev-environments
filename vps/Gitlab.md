
## Pipeline

Connect to your server and install the gitlab runner.

```sh
wget https://gitlab-runner-downloads.s3.amazonaws.com/latest/deb/gitlab-runner_amd64.deb
sudo dpkg -i gitlab-runner_amd64.deb
```

Go to your project on GitLab and get the Runner registration token, visiting a page like:
* https://gitlab.com/company/project/-/settings/ci_cd                              


Register your runner like above:

```sh
sudo gitlab-runner register
# This will show up to be filled
Enter the GitLab instance URL (for example, https://gitlab.com/)
# https://gitlab.com/
Enter the registration token: 
# paste your token
Enter a description for the runner: 
# give a name for your runner, like: teste-server
Enter tags for the runner (comma-separated): 
# test
Enter an executor: 
# docker
Enter the default Docker image (for example, ruby:2.6): 
# ruby:2.7.0
# Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded! 
```

Repeat this process how many times you want to create multiple runners.

You can edit the config using the following:
```sh
cat /etc/gitlab-runner/config.toml
```

You can restart or get the current status running
```sh
sudo gitlab-runner restart
sudo gitlab-runner status
```

### Config Customs

To prevente the runner to always run the docker image from the repo, it will run just once an cache it.
* `pull_policy = ["if-not-present"]`

Example of config.toml
```conf
concurrent = 5
check_interval = 0

[session_server]
  session_timeout = 1800

[[runners]]
  limit = 1
  request_concurrency = 1
  name = "server-teste"
  url = "https://gitlab.com/"
  token = "TOKEN"
  executor = "docker"
  [runners.custom_build_dir]
  [runners.cache]
    [runners.cache.s3]
    [runners.cache.gcs]
    [runners.cache.azure]
  [runners.docker]
    pull_policy = ["if-not-present"]
    tls_verify = false
    image = "ruby:2.7.0"
    privileged = true
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = false
    volumes = ["/cache"]
    shm_size = 0

[[runners]]
  limit = 1
  request_concurrency = 1
  name = "server-test2"
  url = "https://gitlab.com/"
  token = "TOKEN"
  executor = "docker"
  [runners.custom_build_dir]
  [runners.cache]
    [runners.cache.s3]
    [runners.cache.gcs]
    [runners.cache.azure]
  [runners.docker]
    pull_policy = ["if-not-present"]
    tls_verify = false
    image = "ruby:2.7.0"
    privileged = true
    disable_entrypoint_overwrite = false
    oom_kill_disable = false
    disable_cache = false
    volumes = ["/cache"]
    shm_size = 0
```