# doomkin/oracle Dockerfile

Oracle Database 11.2.0.1 with SSH key access on Oracle Linux 6 Dockerfile

This **Dockerfile** is a [trusted build](https://registry.hub.docker.com/u/doomkin/oracle/) of [Docker Registry](https://registry.hub.docker.com/).

### Base Docker Image

* [doomkin/oraclelinux](https://github.com/doomkin/oraclelinux)

### Installation
```
sudo docker pull doomkin/oracle
```

### Run
```
sudo docker run --name orac -d -P doomkin/oracle
```

### Login by SSH
```
ssh-agent -s
ssh-add ssh/id_rsa
ssh root@localhost -p `sudo docker port orac 22 | cut -d":" -f2`
```

### Keep last layer
```
sudo docker export orac > oracle.tar
cat oracle.tar | sudo docker import - doomkin/oracle:raw
```

### To restore metadata rebuild image with Dockerfile
```
FROM doomkin/oracle:raw
EXPOSE 22 1521
CMD service sshd start; \
    export ORACLE_HOME=/opt/oracle/home; \
    export PATH=/opt/oracle/home/bin:$PATH; \
    su oracle -c "lsnrctl start LISTENER"; \
    bash
```
As a result, the size of the Docker image will be 4.7 GB
