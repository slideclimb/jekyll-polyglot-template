Work in progress, don't trust this repo yet :)

---

# Jekyll polyglot template

This template uses:
- Jekyll
- jekyll-polyglot
- MathJax
- GitHub action to deploy generated content to the `gh-pages` branch

To use this template, create a new repo based on the template and follow the steps in [Running locally](#-running-locally).

## Running locally
- Clone your newly-created repo
- Install ruby
- Install bundler
- In the directory where you cloned the repo, run
  - `bundle install` to install the gems listed in the `Gemfile`,
  - `bundle exec jekyll build` to generate the site content in the `_site` directory,
  - `bundle exec jekyll serve` to serve the website on localhost.
- Go to http://127.0.0.1:4000 (or whatever address Jekyll says at the line `Server address`).
- If everything works, you should see something like 

![./example](Blog home page)

## Deploying on GitHub pages
The `pages.yml` action in `.github/workflows` will generate the site content and push it to the `gh-pages` branch. 
We can then deploy the `gh-pages` branch to GitHub pages.

- Make sure the `gh-pages` branch exists by pushing it as an empty branch:
  - Make sure you have no uncommitted changes on the branch you are currently on.
  - `git checkout --orphan gh-pages` to checkout the new branch that does not have any related history to the `main` branch.
  - `git rm -rf .`
  - `git commit --allow-empty -m "initial commit"`
  - `git push origin gh-pages`
- Edit the `.github/workflows/pages.yml`:
  - Make sure that the Action is triggered when you push to your `main` branch (if your main branch is `main` you don't have to do anything) by updating the branch in `on: push: branches: - main`.
  - Set the repository in `jobs: build_and_deploy: steps: with: repository` to the repo you just created.
- In GitHub, go to **Settings > Pages** to configure that GitHub deploys the blog when we (read: our action) push to the `gh-pages` branch:
  - As source, select `Deploy from a branch`
  - As branch, select `gh-pages`
  - Click the save button

To confirm that this works, push something to the main branch and watch the GitHub Actions do their thing!
You can verify that everything works by going to `<username>.github.io/<repo-name>/` and clicking around.
Be especially careful to verify that clicking a link on the `nl` site links to an `nl` page (and not an `en` page). 
If this works, you have successfully set up a Jekyll blog that cannot be deployed by the default GitHub action to deploy a Jekyll blog as that won't allow `jekyll-polyglot`.