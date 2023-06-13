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


# Troubleshoot
How do I delete/unregister a GitLab runner
```
How do I delete/unregister a GitLab runner
```
