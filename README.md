# Bootstrap Errorist Theme for Hugo and Start Blogging

This repository is a skeleton of a Hugo project to be used with Errorist theme. It will pull
Errorist as a Git submodule and also set up a template configuration. This way, you can start your
website project with the minimum of configuration editing.

This repository is meant to be **forked**, not cloned, as it will become your website project which
you probably want to have under _your_ git control. So, fork it first, and then run the following
commands, replacing `<YOUR GITHUB USER NAME>` with, well, your github user name.

```
git clone --recurse-submodules  git@github.com:<YOUR GITHUB USER NAME>/errorist-bootstrap.git
cd errorist-bootstrap
hugo server -D
```

Open `http://localhost:1313` in your browser and follow the instructions.
