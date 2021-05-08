
# Description
This is a repository that is an execution environment for AWX. The purpose of this execution environment  is that it includes PowerShell and also dbatools as a module. DBAtools  1.0.147 is included in the container. 

For the current projects I am working on this is what is needed.

In order to use this execution environment there is two ways that you can use it. 

## docker/podman build
Clone repository and navigate to final_artifacts directory. docker build . -t <tag name>

## Ansible-Builder
create a virtual environment using pip.Switch to the venv
pip3 install ansible-builder==1.0.0.0a1

Clone repository. 
cd into repo. 
chmod +x  run.sh in both the repo working directory and the final_artifacts directory depending on which one you are working with.

If needed make modifications to requirements.yml or requirements.txt to add or remove dependencies catered for your needs.
```
ansible-builder build --tag <docker image> 
```
Docker Image will be ready with necessary dependecies.

NB. There is a line in the execution environment that must not be removed otherwise the container may not work in the AWX environment. This is:

```
    - RUN pip3 uninstall --yes ansible-runner && pip3 install ansible-runner==2.0.0a2
```

The section below comes part of the docker image and should not be removed. 


```
    - COPY --from=quay.io/project-receptor/receptor:0.9.7 /usr/bin/receptor /usr/bin/receptor
    - RUN mkdir -p /var/run/receptor
    - USER 1000
    - ADD run.sh /run.sh
    - CMD /run.sh
    - RUN git lfs install
```
Enjoy!