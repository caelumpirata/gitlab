# Docker image pull from GitLab in Kubernetes

Deploy this *registry-credentials.yml* in the same namespace as of your application 
## How to get token?
```
https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html#create-a-personal-access-token
```

once you have your token, copy keep it somewhere safe
now, run the following command and make sure to Deploy this *registry-credentials.yml* in the same namespace as of your application 
```
kubectl create secret docker-registry registry-credentials --docker-server=registry.gitlab.com --docker-username=<gitlab_username> --docker-password=<token> -n <namespace> --dry-run=client -o yaml > registry-credentials.yml
```
you need to create a new deployment for each namesapce if you have more than one.

## this is how the pipeline code looks
```
deploy to staging:
    stage: deploy staging

    environment: staging

    image: docker:20.10.16
    services:
      - docker:20.10.8-dind

    before_script:  
        - chmod 400 $SSH_KEY
        - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    script:
    - |
      ssh -o StrictHostKeyChecking=no -i $SSH_KEY root@[your_instance_ip_address] "
        docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
        helm install your_char_name -n staging
      "
```
