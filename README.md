Content for my personal blog: http://russ-hyde.rbind.io/

# Environments

The environment for building the website works as follows.

- Rstudio is installed outside of an environment
- A basic conda environment containing the prereqs for opening rstudio is
  activated `conda activate blog-base`
- R packages are specified using {renv}
- Rstudio can be opened from the command line, provided you have activated the
  blog-base environment `rstudio data-globs.Rproj &`

## To activate the blog environment

```
# Activate the conda environment on which r-base is installed
conda activate blog-base

# Then open rstudio, and the 'renv'-defined environment for the project will
# be activated automatically
rstudio data-globs.Rproj &
```

## When working on the blog

In rstudio click "Addins -> Serve Site" or use `blogdown::serve_site()` once

Click "Addins -> New Post" to add a new blog entry

## Notes on converting from full-conda to conda-and-renv

Steps to get the blog building under the renv setup:

- Activate 'blog-base' (add details of R-content of this env)
- `rstudio &` to load global rstudio
- open the blog project "data_blog.Rproj"
- `renv::init()` to initialise an renv environment
- add `renv/library/` to .gitignore (so local package locations/downloads
  aren't pushed up to github)
- Use `install.packages()` until the site can be built. Order of install:
    - blogdown (that blogdown was unavailable was highlighted on opening the
    blog project)
    - miniui
    - rstudioapi [though it dumbfounds me that I have to install miniui/rstudioapi in my        project environment; aren't these for use by rstudio?]
    - [then ran "Serve Site" but got the error "/usr/lib/x86_64-linux-gnu/libstdc++.so.6:       version `GLIBCXX_3.4.26' not found"]
    - [then ran `strings /usr/lib/x86_64-linux-gnu/libstdc++.so.6 | grep
    GLIBCXX` ] which showed that 3.4.26 was not available
    - [then ran `sudo apt-get upgrade libstdc++6`]
    - [that didn't affect the values returned by the strings command]
    - [following this: https://askubuntu.com/questions/575505/glibcxx-3-4-20-not-found-how-to-fix-this-error]
    - [then ran
    ```
    sudo add-apt-repository ppa:ubuntu-toolchain-r/test
    sudo apt-get update
    sudo apt-get upgrade
    ```]
    - [that didn't affect the values returned by the strings command]
    - [then ran `sudo apt-get dist-upgrade`]
    - [now strings includes CXX 3.4.26|27|28 and "Serve Site" throws a "hugo not
    found" error]
  - Ran `blogdown::install_hugo()`
    - This installed v0.81.0 of hugo; and I added options(blogdown.hugo.version =
    "0.81.0") to my .Rprofile
  - Then ran "Serve Site" from rstudio Addins, and the site was served inside
  rstudio