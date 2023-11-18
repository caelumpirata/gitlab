# Pushing images to GitLab Container Registry
inclues codes for pushing image to gitlab registry and how to pull and use it when the repo is private.

* the project is private
* login is mandatory to pull the image

## build stage - single architecture(amd64)
```
# ...
# remaining codes here...

docker-build:
  stage: docker
  image: docker:latest
  services:
    - docker:18.09.7-dind

  script:
    - docker build -t $TARGET_IMAGE:$TARGET_TAG . --load           # creates image and saves it locally
    - docker images
    - docker tag $TARGET_IMAGE:$TARGET_TAG $CI_REGISTRY/hn1674625/gitlab-registry-demo/$TARGET_IMAGE:$TARGET_TAG   # tags image
    - docker images
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker push $CI_REGISTRY/hn1674625/gitlab-registry-demo/$TARGET_IMAGE:$TARGET_TAG      # push to container registry
```
## build stage - multi Architecture (amd64 + arm64)
```
  script:
    - docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
    - docker buildx create --use
    
    - docker buildx build --platform linux/amd64 -t $CI_REGISTRY/hn1674625/gitlab-registry-demo/$TARGET_IMAGE_AMD64:$TARGET_TAG --load .
    - docker buildx build --platform linux/arm64 -t $CI_REGISTRY/hn1674625/gitlab-registry-demo/$TARGET_IMAGE_ARM64:$TARGET_TAG --load .

    - docker images

    - docker push $CI_REGISTRY/hn1674625/gitlab-registry-demo/$TARGET_IMAGE_AMD64:$TARGET_TAG
    - docker push $CI_REGISTRY/hn1674625/gitlab-registry-demo/$TARGET_IMAGE_ARM64:$TARGET_TAG
```

## pull and deploy
```
# ...
# remaining codes here...

deploy to production:
    stage: deploy

    before_script:
        - chmod 400 $SSH_KEY 
    script:
    - |
      ssh -o StrictHostKeyChecking=no -i $SSH_KEY root@[instance_ip_address] "
      docker login -u $CI_REGISTRY_USER -p $CI_REGISTRY_PASSWORD $CI_REGISTRY
      docker pull $CI_REGISTRY/hn1674625/gitlab-registry-demo/$TARGET_IMAGE_AMD64:$TARGET_TAG 
      docker run -d -p 9009:8080 $CI_REGISTRY/hn1674625/gitlab-registry-demo/$TARGET_IMAGE_AMD64:$TARGET_TAG 
      "
```
