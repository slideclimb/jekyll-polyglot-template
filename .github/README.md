Work in progress, don't trust this repo yet :)

---

# Jekyll polyglot template and example

This template uses:
- [Jekyll](https://jekyllrb.com/)
- [jekyll-polyglot](https://polyglot.untra.io/)
- [MathJax](https://www.mathjax.org/)
- [jekyll-deploy-action](https://github.com/jeffreytse/jekyll-deploy-action): A GitHub action to deploy generated content from Jekyll to a git branch.

To use this template, create a new repo based on this template.
Then follow the steps in [Running locally](#-running-locally) to set up your repository and local environment, and follow the steps in [Deploying on GitHub pages](#-deploying-on-github-pages) to set up the automatic deploy.

## Running locally
- Clone your newly-created repo
- Install ruby
- Install bundler
- Edit the `url` and the `baseurl` in the `_config.yml` file.
- In the directory where you cloned the repo, run
  - `bundle install` to install the gems listed in the `Gemfile`,
  - `bundle exec jekyll build` to generate the site content in the `_site` directory,
  - `bundle exec jekyll serve` to serve the website on localhost.
- Go to http://127.0.0.1:4000 (or whatever address Jekyll says at the line `Server address`).
- If everything works, you should see something like 

![example](https://user-images.githubusercontent.com/15685876/184389046-5bfe39cc-a15f-4620-81ec-27027934d36f.png)

## Deploying on GitHub pages
The `pages.yml` action in `.github/workflows` will generate the site content and push it to a given branch.
By default, it is triggered by a push to the `main` branch, and pushes the generated content to the `gh-pages` branch. 
We can then deploy the `gh-pages` branch to GitHub pages.

- Edit the `.github/workflows/pages.yml`:
  - Update the branch in `on: push: branches: ` to the branch you want to push to to trigger the deploy.
  - Set the repository in `jobs: build_and_deploy: steps: with: repository` to the repo you just created.
  - In `jobs: build_and_deploy: steps: with: branch`, set the branch you want the action to push the generated content to.
- Make sure the `gh-pages` branch exists by pushing it as an empty branch:
  - Make sure you have no uncommitted changes on the branch you are currently on.
  - `git checkout --orphan gh-pages` to checkout the new branch that does not have any related history to the `main` branch.
  - `git rm -rf .`
  - `git commit --allow-empty -m "initial commit"`
  - `git push origin gh-pages`
- In GitHub, go to **Settings > Pages** to configure that GitHub deploys the blog when we (read: our action) push to the `gh-pages` branch:
  - As source, select `Deploy from a branch`
  - As branch, select `gh-pages`
  - Click the save button

To confirm that this works, push something to the main branch and watch the GitHub Actions do their thing!
You can verify that everything works by going to `<username>.github.io/<repo-name>/` and clicking around.
Be especially careful to verify that clicking a link on the `nl` site links to an `nl` page (and not an `en` page). 
If this works, you have successfully set up a Jekyll blog that cannot be deployed by the default GitHub action to deploy a Jekyll blog as that won't allow `jekyll-polyglot`.
