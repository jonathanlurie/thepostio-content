---
title: Getting Started
date: 2020-08-22
excerpt: Setup everything and get ready to write content!
cover: cover.jpg
tags:
  - tutorial
  - how-to
---

*Disclaimer:* **The Post** is just at the beginning of its development; bugs may happen, display may be a bit odd. If you fnid anything weird, reach out with the top-left menu! 

# What is ThePost?
**ThePost**, found at `thepost.io`, can be seen as a blog engine, but it's more subtle than that.  
It's a publishing playground where the authors keep full control and ownership of their work. **How?** Simple, we don't store your content, actually, we don't even cache it!

## What do you mean?
ThePost relies on **Github**, if you publish your articles in a certain ways there, they show up here, with a nice display, nice URL to share, SEO support, etc. Note that ThePost is by no means associated with GitHub, it just uses some of its features.

# How do I make content available on ThePost?
Here is the basics, we'll get into the details later:
1. Fork [this repo](https://github.com/thepostio/thepostio-content) (keep the name `thepostio-content`, it's important)
2. Have a look at the structure, you will see it's quite minimalistic
3. Open the file `config.yaml` (in root folder) and replace the values with your own
4. Create a folder in `articles/` for your fist article, using only lowercase letters and dashes instead of spaces. This folder's name will be part of the final URL of your article
5. Create a file `index.md` that contains a YAML front matter
6. Edit the fields of the front matter (between `---` and `---`)
7. Under the second `---`, write your article in markdown syntax
8. Optionaly, if you want this article to show up in your profile page listing, add its folder name in the file `config.yaml` that you can find at the root of this repo
9. Commit and push that on your GitHub repo
10. Visit `https://thepost.io/YOUR_GITHUB_USERNAME/YOUR_ARTICLE_TITLE_SLUG`

**Example:** if your username on GitHub is `johnnybravo` and the folder name of your article (lowercase + dashes) is `my-fabulous-article`, then you can see and share your article at:  
`https://thepost.io/johnnybravo/my-fabulous-article`

And here is how the `index.md` file of your article should look like:
```markdown
---
title: Dummy article
date: 2020-08-23
excerpt: This is a dummy article
cover: dummycover.jpg
tags:
  - test
  - dummy content
---

This is a dummy article.
Lorem ipsum dolor sit amet, consectetur adipiscing elit. Donec vitae bibendum lectus. In hac habitasse platea dictumst. Donec eleifend arcu nisi, eget rhoncus lorem imperdiet in. Pellentesque porta luctus nisi sed rutrum. Duis pharetra ultrices arcu, eu porta risus vehicula vitae. Lorem ipsum dolor sit amet, consectetur adipiscing elit. Fusce nisi dolor, maximus cursus velit non, tincidunt eleifend augue. Quisque elementum dapibus ex, eget bibendum sem lobortis malesuada. Donec convallis condimentum blandit.
```

Note: any image URL (including the `cover` field in the front matter) can be local to your article folder (example: `myimage.jpg`) or absolute (example: `https://example.com/myimage.jpg`)
