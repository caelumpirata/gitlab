# Useful Git Commands to get started with GitLab
![image](https://github.com/caelumpirata/gitlab-springboot/assets/85424262/cb02b3b7-fa3e-48be-839a-8fbfff2c2791)

# Practical use
Inside the empty directory,
```
git init
```
clone your repo from gitlab
```
git clone https://gitlab.com/<your_repo.git>
```
adding files which you want to commit
```
git add <file_name.txt>
```
add all the files at once
```
git add -A
```
commit locally with comment
```
git commit -m "commit text"
```


## Push
pushing file directly to main
```
git push origin main
```


## pushing files in custom branch (other than main)
create new branch (if not exists)
```
git branch <new_branch_name>
```
to push in custom branch other than main
```
git push origin <custom_branch_name>
```

