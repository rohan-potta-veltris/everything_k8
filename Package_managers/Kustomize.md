Kustomize is like an alternative of helm but has more features and stuff

kustomize is a k8 deployment + multi-enviournments 

kustomize doesnt use templates instead they have like a layered approach on each yaml 

in that same root path of my manifest files , we make a "kustomization.yaml" and mention/declare the manifest files used , like the deployments , service , cms etc , and you can add the customization here (example adding a common label)

to run the kustomization 



in the root folder hit find and you can copy and paste the relative files 

kubectl kustomize . => this essentially makes an all in one yaml file 

so you can pipe it with kustomize build path_to_folder | kubectl apply -f -
this will apply all the changes 


now we can also just add it sub-direcory wise , where each folder will have their kustomization and then the in the root call the directories instead of all the files itself 

for example:
in the root:
resources:
- folder1/
- folder2/
- folder3/

and in each of the folder:
resources:
- file1.yaml
- file2.yaml
have it like this 


Now to manage the multiple enviournments we make a folder called ovelays and then dev test prod inside and make the changes required per the enviournemnt 

k8s/
├── base/
│   ├── kustomization.yaml
│   ├── deployment.yaml
│   ├── service.yaml
│   └── configmap.yaml
└── overlays/
    ├── dev/
    │   ├── kustomization.yaml
    │   ├── replica-patch.yaml
    │   └── config.env
    ├── test/
    │   ├── kustomization.yaml
    │   ├── replica-patch.yaml
    │   └── config.env
    └── prod/
        ├── kustomization.yaml
        ├── replica-patch.yaml
        ├── resources-patch.yaml
        └── config.env

Now while applying be careful about the context vs the kubeconfig file being used.
Note in the overlaus kubstomize.yaml we need to pass the base folder

