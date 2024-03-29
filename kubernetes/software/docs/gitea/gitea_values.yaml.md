# gitea_values.yaml

[TOC]



### admin

* ```yaml
  admin:
    #existingSecret: gitea-admin-secret
    username: gitea_admin
    password: r8sA8CPHD9!bt6d
    email: "gitea@local.domain"
  ```
### meteics

* ```yaml
  metrics:
    enabled: false
    serviceMonitor:
      enabled: false
      #  additionalLabels:
      #    prometheus-release: prom1
  ```


### ldap

* ```yaml
  ldap:
    enabled: false
    #existingSecret: gitea-ldap-secret
    #name:
    #securityProtocol:
    #host:
    #port:
    #userSearchBase:
    #userFilter:
    #adminFilter:
    #emailAttribute:
    #bindDn:
    #bindPassword:
    #usernameAttribute:
    #sshPublicKeyAttribute:
  ```

### oauth

* ```yaml
  oauth:
    enabled: false
    #name:
    #provider:
    #key:
    #secret:
    #autoDiscoverUrl:
    #useCustomUrls:
    #customAuthUrl:
    #customTokenUrl:
    #customProfileUrl:
    #customEmailUrl:
  ```

### config

* ```yaml
  config: {}
  #  APP_NAME: "Gitea: Git with a cup of tea"
  #  RUN_MODE: dev
  #
  #  server:
  #    SSH_PORT: 22
  #
  #  security:
  #    PASSWORD_COMPLEXITY: spec
  ```

### podAnnotations

* ```yaml
  podAnnotations: {}
  ```



### database

* ```yaml
  database:
    builtIn:
      postgresql:
        enabled: true
      mysql:
        enabled: false
      mariadb:
        enabled: false
  ```

### cache

* ```yaml
  cache:
    builtIn:
      enabled: true
  ```

### livenessProbe

* ```yaml
  livenessProbe:
    enabled: true
    initialDelaySeconds: 200
    timeoutSeconds: 1
    periodSeconds: 10
    successThreshold: 1
    failureThreshold: 10
  ```

### readinessProbe

* ```yaml
  readinessProbe:
    enabled: true
    initialDelaySeconds: 5
    timeoutSeconds: 1
    periodSeconds: 10
    successThreshold: 1
    failureThreshold: 3
  ```

### startupProbe

* ```yaml
  startupProbe:
    enabled: false
    initialDelaySeconds: 60
    timeoutSeconds: 1
    periodSeconds: 10
    successThreshold: 1
    failureThreshold: 10
  ```

### !other

* ```yaml
  # customLivenessProbe:
  #   httpGet:
  #     path: /user/login
  #     port: http
  #   initialDelaySeconds: 60
  #   periodSeconds: 10
  #   successThreshold: 1
  #   failureThreshold: 10
  # customReadinessProbe:
  #   httpGet:
  #     path: /user/login
  #     port: http
  #   initialDelaySeconds: 5
  #   periodSeconds: 10
  #   successThreshold: 1
  #   failureThreshold: 3
  # customStartupProbe:
  #   httpGet:
  #     path: /user/login
  #     port: http
  #   initialDelaySeconds: 60
  #   periodSeconds: 10
  #   successThreshold: 1
  #   failureThreshold: 10
  ```




## service

* ```yaml
  service:
    http:
      type: ClusterIP
      port: 3000
      clusterIP: None
      #loadBalancerIP:
      #nodePort:
      #externalTrafficPolicy:
      #externalIPs:
      loadBalancerSourceRanges: []
      annotations:
    ssh:
      type: ClusterIP
      port: 22
      clusterIP: None
      #loadBalancerIP:
      #nodePort:
      #externalTrafficPolicy:
      #externalIPs:
      loadBalancerSourceRanges: []
      annotations:
  ```
## ingress

* ```yaml
  ingress:
    enabled: false
    # className: nginx
    annotations: {}
      # kubernetes.io/ingress.class: nginx
      # kubernetes.io/tls-acme: "true"
    hosts:
      - host: git.example.com
        paths:
          - path: /
            pathType: Prefix
    tls: []
    #  - secretName: chart-example-tls
    #    hosts:
    #      - git.example.com
  ```

## resources

* ```yaml
  resources: {}
    # We usually recommend not to specify default resources and to leave this as a conscious
    # choice for the user. This also increases chances charts run on environments with little
    # resources, such as Minikube. If you do want to specify resources, uncomment the following
    # lines, adjust them as necessary, and remove the curly braces after 'resources:'.
    # limits:
    #   cpu: 100m
    #   memory: 128Mi
    # requests:
    #   cpu: 100m
    #   memory: 128Mi
  
  ## Use an alternate scheduler, e.g. "stork".
  ## ref: https://kubernetes.io/docs/tasks/administer-cluster/configure-multiple-schedulers/
  ##
  # schedulerName:
  ```

## nodeSelector: {}

## tolerations: []

## affinity: {}

## statefulset

* ```statefulset:
  env: []
    # - name: VARIABLE
    #   value: my-value
  terminationGracePeriodSeconds: 60
  labels: {}
  ```
## persistence
* ```
  persistence:
  enabled: true
  # existingClaim:
  size: 10Gi
  accessModes:
    - ReadWriteOnce
  labels: {}
  annotations: {}
  # storageClass:
  # additional volumes to add to the Gitea statefulset.
  ```
## extraVolumes
* ```yamlextraVolumes:
  # - name: postgres-ssl-vol
  #   secret:
  #     secretName: gitea-postgres-ssl
  
  # additional volumes to mount, both to the init container and to the main
  # container. As an example, can be used to mount a client cert when connecting
  # to an external Postgres server.

## extraVolumeMounts

* ```yaml
  # - name: postgres-ssl-vol
  #   readOnly: true
  #   mountPath: "/pg-ssl"
  
  # bash shell script copied verbatim to the start of the init-container.

## initPreScript

* ```yaml
  initPreScript: ""
  #
  # initPreScript: |
  #   mkdir -p /data/git/.postgresql
  #   cp /pg-ssl/* /data/git/.postgresql/
  #   chown -R git:git /data/git/.postgresql/
  #   chmod 400 /data/git/.postgresql/postgresql.key
  
  # Configure commit/action signing prerequisites
  ```

## signing

* ```yaml
  signing:
    enabled: false
    gpgHome: /data/git/.gnupg
  ```
## memcached
* ```yaml
  memcached:
    service:
      port: 11211
  ```

## database 三选一

### postgresql
* ```yaml
  postgresql:
    global:
      postgresql:
        postgresqlDatabase: gitea
        postgresqlUsername: gitea
        postgresqlPassword: gitea
        servicePort: 5432
    persistence:
      size: 10Gi
  ```
### mysql
* ```yaml
    mysql:  
    root:
      password: gitea
    db:
      user: gitea
      password: gitea
      name: gitea
    service:
      port: 3306
    persistence:
      size: 10Gi
  ```
### mariadb
* ```yaml
  mariadb:
    auth:
      database: gitea
      username: gitea
      password: gitea
      rootPassword: gitea
    primary:
      service:
        port: 3306
      persistence:
        size: 10Gi

