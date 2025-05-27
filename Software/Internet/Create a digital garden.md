---
publish on andju-knows: true
---
# Create a digital garden
This tutorial is based on the work of [Daniel Nazarian](https://www.danielnazarian.com/blog/posts/0d7a916e-cd8f-4931-82a5-f206ab1a938e/), who did a great job in developing a digital garden based on Obsidian and GitHub Pages. He could have done a better job in documenting it beginner-friendly 😉.

1. Create a GitHub repository. If you are using GitHub for free, the repository must be public
2. Configure the following repository setting: Settings ➡️ Actions ➡️ General ➡️ Workflow permissions ➡️ Read and write permissions
3. Clone your repository and add the following files from [Dan's Repository](https://github.com/dan1229/tutorial-obsidian-mkdocs-self-hosted):
	1. `deploy.yml` into the new subfolder `.github\workflows`
	2. `mkdocs.yml.basic` or `mkdocs.yml.adv` into the root (remove the `.basic` resp. `.adv` extension)
	3. `preprocess_dataviews.py` into the new subfolder `.scripts`
	4. Create an (empty) `extra.css` file in the new subfolder `.overrides` (otherwise the workflow will fail)
4. Create an index.md file
5. Commit the new files to GitHub
6. Ensure that the GitHub workflow runs successfully (Actions tab in your repository)
7. Configure the following repository settings in Settings ➡️ Pages ➡️ Build and deployment: 
		1. `Source`: Deploy from a branch
	1. `Branch`:
		1. `gh-pages`
		2. `/ (root)`