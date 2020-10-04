---
title: "add jekyll in vue project"
subtitle: "setting gemfile"
layout: post
author: "libyasdf"
header-style: text
tags:
  - jekyll
  - Gemfile
---
# vue-cli
bulid a vue project on master branch with vue-cli

# gh-pages branch
```
rm -rf *
```
delete all files in this branch
```
git checkout --orphan gh-pages
git push --set-upstream origin gh-pages
jekyll new docs
```

# 配置

###### issues fixed
[Could not find gem github-pages](https://github.com/prose/starter/issues/44)  


>specify the version for gem "github-pages" in GEMFILE:<br/>
gem "github-pages", "~> 203", group: :jekyll_plugins<br/>
replace the version ( 203 ) above to the one from https://pages.github.com/versions/<br/>
then run:<br/>
>```
>bundle update jekyll
>```
>followed by :
>```
>bundle install
>```
>test site by launching it locally :
>```
>bundle exec jekyll serve
>```

[githubpages Dependency versions](https://pages.github.com/versions/)  
[github creating a gh-pages with jekyll](https://docs.github.com/en/free-pro-team@latest/github/working-with-github-pages/creating-a-github-pages-site-with-jekyll)  
