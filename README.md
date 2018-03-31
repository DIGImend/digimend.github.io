DIGImend website
================

This is the DIGImend project website, normally accessible
at [http://digimend.github.io](http://digimend.github.io).

After cloning this repository, run

    git submodule init

followed by

    git submodule update

to checkout the tablets submodule repository.

Install ruby-dev, and zlib1g-dev

    sudo apt-get install ruby-dev zlib1g-dev

Install Bundler:

    gem install bundler

Install Jekyll and other dependencies from the GitHub Pages gem:

    bundle install

Run your Jekyll site locally:

    bundle exec jekyll serve

and open the printed URL (usually [http://0.0.0.0:4000/](http://0.0.0.0:4000/)).
