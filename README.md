# GitHub Action: Creates Pull Request when Submodules are Updated

This GitHub action creates a new branch and pull request against the parent repository when submodules are updated

**The end goal of this tool:**

- Create a new branch on the parent repository and get all submodules updates
- Create a pull request from newly created branch

## How to use

To use this **GitHub** Action you will need to complete the following in the submodule repository:

1. Parent repository has `.gitmodules` configured
2. Store GitHub token into secrets with permissions to write to parent repository
3. Create a new file in your repository called `.github/workflows/submodule-update.yml`
4. Copy the example workflow from below into that new file and modify as needed
5. Commit that file to a new branch
6. Observe the action working

This file should have the following code:

```yml
---
name: Submodule Updates

#############################
# Start the job on all push #
#############################
on:
  push:
    branches: [develop]

###############
# Set the Job #
###############
jobs:
  build:
    name: Submodule update
    runs-on: ubuntu-latest
    env:
      PARENT_REPOSITORY: 'org/example-repository'
      CHECKOUT_BRANCH: 'develop'
      PR_AGAINST_BRANCH: 'develop'
      OWNER: 'org'

    steps:
      ##########################
      # Checkout the code base #
      ##########################
      - name: Checkout Code
        uses: actions/checkout@v4

      ####################################
      # Run the action against code base #
      ####################################
      - name: run action
        id: run_action
        uses: releasehub-com/github-action-create-pr-parent-submodule@v4
        with:
          github_token: ${{ secrets.RELEASE_HUB_SECRET }}
          parent_repository: ${{ env.PARENT_REPOSITORY }}
          checkout_branch: ${{ env.CHECKOUT_BRANCH}}
          pr_against_branch: ${{ env.PR_AGAINST_BRANCH }}
          pr_title: 'Optional title to override the default'
          pr_description: 'Optional description to override the default'
          owner: ${{ env.OWNER }}
```

Working example from the CanDIG project: [CanDIG/candigv2-ingest workflow file](.github/workflows/example.yml) 

## Acknowledgements

This action builds on prior work by:
* [@justin-ys](https://github.com/justin-ys) 
  * [justin-ys/github-action-pr-expanded](https://github.com/justin-ys/github-action-pr-expanded)
* [@releasehub-com](https://github.com/releasehub-com)
  * [releasehub-com/github-action-create-pr-parent-submodule](https://github.com/releasehub-com/github-action-create-pr-parent-submodule)
  * [GitHub Marketplace](https://github.com/marketplace/actions/github-action-submodule-updates)
