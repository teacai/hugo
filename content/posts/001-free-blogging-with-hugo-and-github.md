---
title: "10 Steps to free blogging with Hugo and GitHub Pages"
date: 2020-12-01T11:09:46Z
draft: false
categories: [blog, publishing, guide]
tags: [hugo, content, static site generator, github]
---
# 10 Steps to free blogging with Hugo and GitHub Pages
Blog like a developer: with [Hugo](https://gohugo.io/) and [Markdown](https://www.markdownguide.org/getting-started/) via your [GitHub](https://github.com) repository in 10 easy steps.
This is a quick guide to your own static blog hosted on GitHub Pages and served on your own domain.

#### Hugo
This quick start guide is based on [Hugo](https://gohugo.io/getting-started/quick-start/), check their documentation for more configuration options and troubleshooting.

#### 1. Install Hugo
```
brew install hugo
# or
port install hugo
```

#### 2. Create new blog
```
hugo new site hugo-blog
```
#### 3 Customize your blog for GitHub Pages
Edit the `config.toml` file change the title as you wish if you intend to use a custom domain you may want to update the `baseURL` as well.
To be able to publish via GitHub Pages add the following line to the file:
```
publishdir = "docs"
```
To add a theme change the `config.toml` adding the line:
```
theme = "ananke"
```
You also need to copy the theme files to `themes` or you could add the theme git repository as submodule:
```
# in hugo-blog directory:
git init
git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke
```

Also create the file `static/CNAME` with contents your custom domain (i.e `teaca.eu`).

#### 4. New post
```
cd hugo-blog
hugo new posts/my-first-post.md
```
Modify the file `posts/my-first-post.md` to write your first post. To publish the post change the `draft:` to  `false`. 
You may also add tags and categories to the post like:
```
categories: [test, publishing]
tags: [hugo, content, static site generator]
```

#### 5. Add to git
Create your git repository `hugo-blog`
```
# git init only if you haven't already done that adding the theme
git init

git remote add origin git@github.com:[your-git-user]/hugo-blog.git
```

#### 6. Build to publish with a git pre-commit hook
To build the pages you need just to run `hugo` command and your static site will be built in `docs` folder. 

Of course, it would be much nicer to have the blog published automatically with the new content we add via a commit and we can do that easily with a git pre-commit hook.
To add the hook, rename the file `pre-commit.sample` in `pre-commit` (location: `hugo-blog/.git/hooks`) and replace the contents of the file with: 
```
#!/bin/sh
#
# Run Hugo build and add the generated files to the commit
#
echo "Pre-commit hook start..."

echo "Building Hugo distribution"
hugo

echo "Adding new generated files to commit"
git add docs/*

echo "Pre-commit hook done."
```

#### 7. Test
To test how your site looks run ``hugo server -D`` (`-D` enables drafts to be shown).
Now you can go to http://localhost:1313/ to check your blog. Ctrl+C to stop the server.

#### 8. Commit your changes
Commit your changes and push them to `main` branch:
```
git add *
git commit -m "First post"
git push origin HEAD:main
```

#### 9. Configure GitHub pages
Go to your repository page on **GitHub > Settings**, scroll down to **GitHub Pages** section and set the `source` as `main` and folder `/docs` > _Save_ then repeat, now setting **Custom domain**.

#### 10. Configure DNS
Use your registrar's DNS manager or to your [Cloudflare](https://cloudflare.com) DNS management page and add a CNAME entry pointing to the url showing in **Custom domain** section:
```
CNAME @ [your-git-user].github.io 
```

That's it, navigate to your custom domain to see your Hugo blog.
You might already notice that this blog is hosted in the exact way described above. 