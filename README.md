# Repo struture
-> .github github specific files
-> .jenkins jenkins pipeline scripts
-> application source code
    -> source files
    -> integration tests
    -> Dockerfile for building the app
    -> helm charts for kubernates deployement
-> Infrastructure as code
    -> terraform scripts for cloud
    -> Kubernetes manifests
-> utility scripts db migrations
-> ignore certain files
-> Jenkins Pipeline definition        

# Git Branching Strategy

-> Main branch 
    ->Represents the production ready state
    ->protected branch (no direct commits)
    ->Only updated via pull requests(PRS) from relese or hotfix branches

-> Develop Branch
    ->represent the latest development state
    ->all features are merged here
    ->used for staging and pre-production testing

-> Feature Branch 
    ->created from develop for new feature or bug fixes
    ->naming convention feauture/ name or fix/ name
    ->Merged back into develop after code review and testing

-> Release Branch 
    ->created from develop for preparing a production release
    ->Naming convention  release/version
    ->Merged into main and develop after testing

-> Hotfix branches
    ->created from main for urgent production fixes
    ->Naming convention hotfix/issuename
    ->Merged into main and develop after testing

# Jenkins CI PipeLine

-> Jenkins will Handle the continious integration process.use a Jenkins file in the root of the repo to define the pipeline

# Argo CD for Continious Deployment

-> Argo CD will sync the Kubernetes manifests or helm charts with the desired state in the git repo.
   ->store K8 manifests or Helm charts in the app/helm or infra/kubernetes
   ->Configure Argo CD to monitor the main branch for production deployments and the develop branch for staging deployments.
   ->use the Argo cd Application CRD to define the deployment target.

# WorkFlow Overview

-> Development
   ->Developers create feauture branches from develop
   ->push chenges and create PR'S tp merege into develop
   ->Jenkins runs CI pipeline on PR'S to develop.   

-> Staging 
   ->After Merging to develop,Jenkins builds and tests the application
   ->Argo CD deploys the develop branch to the staging environment.

-> Production 
   -> Create a release branch from develop
   -> perform final testing on the release branch
   -> Merge release into main and tag the  release
   -> Argo CD deploys the main branch to the production environment

-> Hotfixes
   -> Create a hotfix branch from main
   -> Merge the hotfix into main and develop after testing

# Best Prsctices
   -> Code Reviews: Require PR reviews before merging into develop or main
   -> Automated Testing: Ensure jenkins runs unit,integration and end-to-end tests.
   -> Immutable Releases:Use unique tags for Docker images and helm charts
   ->Monotirng:Integrate montoring tools(Prometheus,Grafana) for production deployments.
   ->Security: Scan Docker Images and dependenchies for vunerabilities in the CI pipeline.