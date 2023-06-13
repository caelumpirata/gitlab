# using local runner for gitlab
>> install docker first 
```
apt install docker.io
```
* follow step 1 to 3, from this 👇
```
https://docs.gitlab.com/runner/install/linux-manually.html#update-1
```
* next step 😀
* goto Project setting >> CI/CD settings >> runners 
* create new runner 
* add 2 tags
* copy and paste generated command 
```
gitlab-runner register  --url https://gitlab.com  --token <xxxxxxxxxxxxxxx>
```
* choose docker 
* choose default *ruby...* for docker image

later-------------
* edit you newly created runner, and tick only these..
![image](https://github.com/caelumpirata/gitlab-springboot/assets/85424262/4d30e8f8-e734-488a-848a-86fc28d65223)


# T R O U B L E S H O O T

## How do I delete/unregister a GitLab runner
```
How do I delete/unregister a GitLab runner
```
## Error during connect: Post http://docker:2375/v1.40/auth: dial tcp: lookup docker on 169.254.169.254:53: no such host
```
https://forum.gitlab.com/t/error-during-connect-post-http-docker-2375-v1-40-auth-dial-tcp-lookup-docker-on-169-254-169-254-no-such-host/28678/5
```
![image](https://github.com/caelumpirata/gitlab-springboot/assets/85424262/c2440bc9-e5b1-4bf9-95f7-9e35ce8ff683)
![image](https://github.com/caelumpirata/gitlab-springboot/assets/85424262/553e5291-f209-4d72-9d88-e11ec4a9dbcc)
<img width="335" alt="image" src="https://github.com/caelumpirata/gitlab-springboot/assets/85424262/83c92c6f-15f1-4424-875d-fdc4ac53829a">
```
docker-build:
    stage: docker
    image: docker:latest
    services:
      - docker:18.09.7-dind
    tags:
      - worker
    variables:
      DOCKER_HOST: tcp://docker:2375/
      DOCKER_TLS_CERTDIR: ""
    before_script:  
      - docker login -u $REGISTRY_USER -p $REGISTRY_PASS
    script:
      - docker build -t $APP_IMAGE .
      - docker push $APP_IMAGE

```
