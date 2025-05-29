---
publish on andju-know: true
---
# Digital gardening
What is a digital garden?

According to [Anne-Laure Le Cunff](https://nesslabs.com/digital-garden-set-up):
> A digital garden is an online space at the intersection of a notebook and a blog, where digital gardeners share seeds of thoughts to be cultivated in public. Contrary to a blog, where articles and essays have a publication date and start decaying as soon as they are published, a digital garden is evergreen: digital gardeners keep on editing and refining their notes.

According to me:
> A way to collect, manage and share information for people who don't want to depend on social media companies or maintain a blog.
## The easy way
[Obsidian Publish](https://obsidian.md/publish) is the most straightforward and convenient option to create your digital garden. You only need Obsidian (see [[Recommended Software]]).
## The advanced way
If you are looking for a free and/or more customizable solution, I recommend the approach of [Daniel Nazarian](https://www.danielnazarian.com/blog/posts/0d7a916e-cd8f-4931-82a5-f206ab1a938e/), which is based on Obsidian and GitHub Pages.

You need:

- A (free) GitHub account
- A git client (e.g. [Git for Windows](https://gitforwindows.org/))
- Obsidian (see [[Recommended Software]])
### Prepare GitHub
1. Create a GitHub repository (if you are using GitHub for free, the repository must be public)
2. Settings ➡️ Actions ➡️ General ➡️ Workflow permissions ➡️ Read and write permissions
3. Clone your repository and add the following files from [Dan's Repository](https://github.com/dan1229/tutorial-obsidian-mkdocs-self-hosted):
	1. `deploy.yml` into the new sub-folder `.github\workflows`
	2. `mkdocs.yml.basic` or `mkdocs.yml.adv` into the root-folder (remove the `.basic` resp. `.adv` extension)
	3. `preprocess_dataviews.py` into the new sub-folder `.scripts`
	4. Create an (empty) file (e.g. `extra.css`) file in the new sub-folder `.overrides` (otherwise the workflow will fail)
4. Commit & Push the files
### Add content
1. Open the folder with the cloned repository as a Vault in Obsidian
2. Create a page named index in the root and add some content
3. Commit & Push the `index.md` file
4. Ensure that the GitHub workflow in your repository runs successfully (Actions tab)
### Configure GitHub Pages
In your GitHub repository: Settings ➡️ Pages ➡️ Build and deployment:

- `Source`: Deploy from a branch
- `Branch`: `gh-pages` and  `/ (root)`
### Enhance your digital garden
Congratulations: The digital garden is ready - now make it yours! I found the following tweaks helpful:

- Add a `.gitignore` file (see [example](https://publish.obsidian.md/git-doc/Tips-and-Tricks#Gitignore)) to avoid adding unnecessary files to your repository.
- Dan uses the \#publish-me tag to mark pages to be published, which mkdocs turns into a header. Therefore I add a property with a unique name (like `publish-page: true`) at the beginning of each page and replaced the occurrences of \#publish-me in `deploy.yml` with it. You can also add the property (together with other default elements) to a [template](https://help.obsidian.md/plugins/templates) (Note: The folder "Templates" is already excluded in `deploy.yml`).
- In order to support the Obsidian features [Wikilinks](https://help.obsidian.md/links) and [callouts](https://help.obsidian.md/callouts), add the [mkdocs-obsidian-bridge](https://pypi.org/project/mkdocs-obsidian-bridge/) package to `deploy.yml` and the respective options to `mkdocs.yml`.
- The [Obsidian Git Plugin](https://github.com/Vinzent03/obsidian-git) allows you to push your updates without leaving Obsidian.

### Useful Links

- [Supported language shortcodes (for code blocks)](https://pygments.org/docs/lexers/)