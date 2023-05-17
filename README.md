# How to use this template

## Environments setup
### **Create GitHub repositories**
Use this repo as template to create the following repos:

- a private repo that will host the source code
- a public repo that will be used to publish the dev version of the website
- a public repo that will be used to publish the prod version of the website

### **Initialize secrets and variables**

### In the private repo hosting the source code
### Create a **Secret**
*Settings > Secrets and variables > Actions > Secrets > New repository secret*

    Secret name: DISPATCH_TOKEN
    Value: <GitHub Personal Access Token>

    Token permissions:
    - repo:public_repo


### Create **variables**:

*Settings > Secrets and variables > Actions > Variables > New repository variable*
### dev environment:
    Variable name: GH_PAGES_DEV_REPO
    Value: <GitHub org>/<Gihub Pages Dev repo>
    
    This will be the public repo where the dev version of the website will be published on GitHub Pages.
    example: `my-org/my-website-dev-public`

    The website will be published at the following URL: https://<GitHub org>.github.io/<Gihub Pages Dev repo>

####  

### prod environment:
    Variable name: GH_PAGES_PROD_REPO
    Value: <GitHub org>/<Gihub Pages Prod repo>
    
    This will be the public repo where the prod version of the website will be published on GitHub Pages.
    example: `my-org/my-website-prod-public`

    The website will be published at the following URL: https://<GitHub org>.github.io/<Gihub Pages Prod repo>


--- 

### In each Public repo hosting GitHub Pages (Dev and Prod)
### Create a **Secret**
    
    Secret name: GH_PAGES_TOKEN
    Value: <GitHub Personal Access Token>
    
    Token permissions:
    - repo:all


## How to use
Clone the private repo hosting the source code and start working on it.

When you are ready to publish the dev version of the website, push your changes to the `public/dev` branch of the private repo. The GitHub Action workflow defined in file `.github/workflows/publish-to-dev.yml` will be triggered and will POST a `repository_dispatch` event to the dev public repo. The payload of the event will contain the following json:
```json
{
  "repository": "<name of the private repo hosting the source code>",
  "branch": "public/dev",
  "message": "Dispatch event sent from github_org/branch" 
}
```


This event will trigger the GitHub Action workflow defined in file `.github/workflows/publish-gh-pages.yml`. This workflow is doing the following:
- clone the private repo provided in the dispatch event payload (using the `DISPATCH_TOKEN` to access the private repo)
- Setub GitHub pages for the dev repo
- Upload all content of `src/public` directory as artifacts
- publish artifacts to GitHub Pages


The same process is used to publish to prod, simply push your changes to the `public/prod` branch of the private repo.