# New Deployment flow


## Typical flow
1. Create branch from dev
2. Add commits
3. Push branch on remote
4. Create & Merge PR into dev

5. Make Release branch
6. Pick commits from dev
7. Push branch to remote
8. Add tag `vX.X.X` and push to remote

OR

8. Add tag `vX.X.X` to dev commit and push to remote

9. In GitHub actions pick `Deploy` workflow. Add version as input `vX.X.X``, env stage
10. Action will create Stage deployment PR.
11. Approve it and merge
12. Once merged Build & Deploy task will run and deploy data to Stage. Also stage/latest tag will be updated and `stage/vX.X.X` created.

13. In GitHub actions pick `Deploy` workflow. Add version as input `vX.X.X`, env prod
14. Action will create Prod deployment PR.
15. Approve it and merge
16. Once merged Build & Deploy task will run and deploy data to Prod. Also `prod/latest` tag will be updated and `prod/vX.X.X` created.

## Hotfix flow
1. Create tag `vX.X.X-hotfixX` from the hotfix branch 
2. In GitHub actions pick `Deploy` workflow. Add version as input `vX.X.X-hotfixX`, env stage
10. Action will create Hotfix deployment PR.
11. Approve it and merge
12. Once merged Build & Deploy task will run and deploy data to Stage and prod. Also stage/latest and prod/latest tags will be updated and `stage/vX.X.X-hotfixX`, `prod/vX.X.X-hotfixX` created.
