# sonarqube

![Sonarqube](https://wiki.eclipse.org/images/8/88/Sonarqube.png)

```sh
docker-compose up
```

```
ERROR: [1] bootstrap checks failed. You must address the points described in the following [1] lines before starting Elasticsearch.
bootstrap check failure [1] of [1]: max virtual memory areas vm.max_map_count [65530] is too low, increase to at least [262144]
```

Increase the vm.max_map_count sysctl default value from the Linux default `65530` to `262144`

```sh
podman machine ssh
sudo sysctl -w vm.max_map_count=262144
```

```sh
docker-compose up -d
```

```sh
$ docker-compose logs sonarqube | grep 'operational'
2024.02.11 23:32:27 INFO  web[][o.s.s.p.Platform] Web Server is operational
2024.02.11 23:32:30 INFO  app[][o.s.a.SchedulerImpl] SonarQube is operational
```

URL: http://localhost:9000
Username: admin
Password: admin

[](https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html#_set_vm_max_map_count_to_at_least_262144)


## SonarQube Scanner

```sh
docker pull docker.io/sonarsource/sonar-scanner-cli

docker run --rm -it -v ${PWD}:/usr/src \
        --network host \
        --name sonar-scanner \
        -e SONAR_HOST_URL="http://localhost:9000" \
        -e SONAR_LOGIN="TOKEN" \
        docker.io/sonarsource/sonar-scanner-cli
```

## References

 - [ติดตั้ง SonarQube Server ด้วย Docker Compose](https://codinggun.com/sonarqube/install-docker/)
 - [Fedora - Changes/IncreaseVmMaxMapCount](https://fedoraproject.org/wiki/Changes/IncreaseVmMaxMapCount)
