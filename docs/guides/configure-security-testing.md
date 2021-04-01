# Security Testing

There are 2 main application security testing methodologies, **SAST** (Static Analysis Security Testing) helps to find software flaws and weaknesses by code scanning and **DAST** (Dynamic Analysis Security Testing) which takes a black-box approach trying find vulnerabilities that could be exploited while the application is running.  

## Static Analysis 

Implementation done as part of the pipeline 

- Trivy [aquasecurity/trivy](https://github.com/aquasecurity/trivy) for container images scanning.
- Helm Chart analysis , [Fairwinds/Polaris](https://insights.docs.fairwinds.com/reports/polaris/).
- Dependabot


## Dynamic Analysis 

Example how this could be implemented using zaproxy as docker compose service.

### Dockerfile 
```
FROM  owasp/zap2docker-stable:2.10.0

ENV ZAP_PORT 8095

ENTRYPOINT  zap.sh -daemon \
 -port ${ZAP_PORT} \
 -host 0.0.0.0 \
 -config api.addrs.addr.name=.* \
 -config api.addrs.addr.regex=true \
 -config api.disablekey=true

```


### Service ( docker-compose ) 
```
version: "3.8"
services:
  owasp-zaproxy:
    build:
      context: ../../
      dockerfile: infra/owasp/zaproxy/Dockerfile
    image: zaproxy
    hostname: zaproxy
    environment:
      ZAP_PORT: 8095

    networks:
      - integrity

    ports:
      - 8095:8095

    healthcheck:
      test: [ "CMD", "zap-cli", "-p", "8095", "status" ]
      interval: 15s
      timeout: 5s
      retries: 3

networks:
  integrity:
    external:
      name: integrity

```

### Execute Full Scan 

1. Start zap proxy service 
```shell
docker-compose up -d 
```

2. Open a URL using the ZAP proxy
```shell
docker-compose exec owasp-zaproxy zap-cli open-url http://target-ui:8080/ui 
```

3. Execute full scan
```shell
docker-compose exec owasp-zaproxy zap-full-scan.py -t http://target-ui:8080/ui
```



