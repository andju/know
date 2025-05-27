---
publish on andju-know: true
---
# Create a digital garden
This tutorial is based on the work of [Daniel Nazarian](https://www.danielnazarian.com/blog/posts/0d7a916e-cd8f-4931-82a5-f206ab1a938e/), who did a great job in developing a digital garden based on Obsidian and GitHub Pages. He could have done a better job in documenting it beginner-friendly 😉.

## Set up
You need:

- A (free) GitHub account
- A git client on your platform (e.g. [Git for Windows](https://gitforwindows.org/))
- Obsidian (see [[Recommended Software]])
### Prepare GitHub
1. Create a GitHub repository (if you are using GitHub for free, the repository must be public)
2. Settings ➡️ Actions ➡️ General ➡️ Workflow permissions ➡️ Read and write permissions
3. Clone your repository and add the following files from [Dan's Repository](https://github.com/dan1229/tutorial-obsidian-mkdocs-self-hosted):
	1. `deploy.yml` into the new subfolder `.github\workflows`
	2. `mkdocs.yml.basic` or `mkdocs.yml.adv` into the root (remove the `.basic` resp. `.adv` extension)
	3. `preprocess_dataviews.py` into the new subfolder `.scripts`
	4. Create an (empty) `extra.css` file in the new subfolder `.overrides` (otherwise the workflow will fail)
4. Commit & Push the files
### Add content
1. Open the folder with the cloned repository as a Vault in obsidian
2. Create a page named index in the root and add some content
3. Commit & Push the file
4. Ensure that the GitHub workflow in your repository runs successfully (Actions tab)
### Configure GitHub Pages
In your GitHub repository: Settings ➡️ Pages ➡️ Build and deployment:

- `Source`: Deploy from a branch
- `Branch`: `gh-pages` and  `/ (root)`
## Enhancements
Dan uses the \#publish-me tag to mark pages to be published, which mkdocs turns into a header. Therfore I add a property with a unique name (like `publish-page: true`) at the beginning of each page and replaced the occurrences of \#publish-me in `deploy.yml` with it.