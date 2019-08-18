# nerva-docs
Official NERVA documentation (https://docs.getnerva.org)

Copyright (c) 2019 The NERVA Project

## Contributing
All you need to help us write the docs is a basic knowledge of the [Markdown formatting language](https://www.markdowntutorial.com/), which is easy to learn. To contribute you'll need to fork this repository, make your desired changes and then submit a pull request to merge those changes. If you aren't familiar with this process, feel free to ask for help on Discord: https://discord.gg/xBHxnGN

Contributions may not immediately appear on the website, as the docs must first be rebuilt (into HTML) via mkdocs.

## Building the docs locally
    $ pip install mkdocs mkdocs-git-committers-plugin
    $ git clone https://github.com/mrsyzygy/nerva-docs.git
    $ cd nerva-docs
    $ git clone https://github.com/mrsyzygy/mkdocs-bootstrap4.git
    $ mkdocs build
