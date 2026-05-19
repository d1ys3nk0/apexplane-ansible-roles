# GitLab Runner

Create a runner token in GitLab, then register the runner:

```sh
docker run --rm -it \
  -v /opt/gitlab-runner/config:/etc/gitlab-runner \
  gitlab/gitlab-runner register \
  --non-interactive \
  --description "$(hostname -f)" \
  --executor docker \
  --docker-image docker:latest \
  --docker-volumes /var/run/docker.sock:/var/run/docker.sock \
  --url "<GITLAB_URL>" \
  --token "<RUNNER_TOKEN>"
```

Tune `/opt/gitlab-runner/config/config.toml`, then restart:

```sh
docker restart gitlab-runner
```

Useful baseline:

```toml
concurrent = 4
check_interval = 0
shutdown_timeout = 0

[session_server]
  session_timeout = 1800
```
