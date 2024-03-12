test12-dep111

# Dev to prod steps

## 1. Local developer machine
git checkout dev
git pull
git checkout -b developer/feature dev
git commit ..
git push .. >>>>

## 2. Creating PR
1. Developer create PR to the Dev branch
2. ACTION: Assigning reviewers
3. ACTION: Check approvals
4. ACTION: CI Tests
5. Approved by assigned people >>>>

## 3. Dev [On Push ACTION] 
1. Builing the image with -dev-hash sufix
2. Pushing image to the ECR
3. Pushing update to the k8s Dev repository >>>>
4. Checking deploy status (if possible)

## 4. K8S DEV [On Push ACTION] 
1. Update k8s Dev cluster

## 5. Creating Release PR
1. Creating releases/vX.X.X branch from some dev commit to the Stage branch
   (version set manually and used in flow till the prod)
2. ACTION: Assigning reviewers
3. ACTION: Check approvals
4. ACTION: CI Tests
5. Approved by assigned people >>>>

## 6. Stage [On Push ACTION from releases/*]
1. Creating tag with version parsed from the version in branch name
2. Builing the image with -vX.X.X sufix
3. Pushing image to the ECR
4. Pushing update to the k8s Stage repository >>>>
5. Checking deploy status (if possible)

## 7. K8S STAGE [On Push ACTION] 
1. Update k8s Stage cluster

## 8. Deploy on prod [Workflow Run] : input[version]
1. Auto-Creating PR from set version

## 9. Deploy Prod PR
1. ACTION: Assigning reviewers
2. ACTION: Check approvals
4. Approved by assigned people >>>>

## 10. Prod [On Push ACTION]
4. Pushing update to the k8s Stage repository >>>>
5. Checking deploy status (if possible)

## 11. K8S DEV [On Push ACTION] 
1. Update k8s Prod cluster

# Hotfix

## 12. Creating Hotfix PR
1. Creating branch hotfixes/vX.X.X+1 (version the same that was deployed + 1)
2. Adding code to fix
3. Creating PR to the Stage branch
4. ACTION: Assigning reviewers (different list then for ordinary deploy)
5. ACTION: Check approvals
6. ACTION: CI Tests
7. Approved by assigned people >>

## 13. Stage [On Push ACTION from hotfixes/*]
1. Creating tags stage/vX.X.X+1, prod/vX.X.X+1 with version parsed from the version in branch name
2. Builing the image with -vX.X.X+1 sufix
3. Pushing image to the ECR
4. Pushing update to the k8s Stage repository >>>>
5. Pushing update to the k8s Prod repository >>>>
4. Pushing update to the Prod branch
5. Checking deploy status (if possible)


# Questions 

## Why we need releases branches?
In the same way how it described in the Git Flow. We need them to not block dev.
For example we may have some commits that shouldn't got to the current release.

## Can we get rid of branches?
No, as we running all our approvals in the PR itself. But Stage and Prod 
branches not used for anything except saving the code of the last deployed 
version. Something like prod/latest tag.
I could make them temporarily, but problem arising with `ON PUSH` event
which i need for running deployment, and wich couldnt be filtered by branch 
from which push is made (Only set if inside the action, but the action still 
will be running)
