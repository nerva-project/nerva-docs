# nerva-docs
Official NERVA documentation (https://docs.nerva.one)

Copyright (c) 2018 - 2024 NERVA Project

## Contributing
All you need to help us write the docs is a basic knowledge of the [Markdown formatting language](https://www.markdowntutorial.com/), which is easy to learn. To contribute you'll need to fork this repository, make your desired changes and then submit a pull request to merge those changes. If you're unfamiliar with this process, feel free to ask for help on [Discord][nerva-discord-link].

Contributions may not immediately appear on the website, as the docs must first be built (into HTML) via mkdocs.

## Building the docs locally
Requires [python/pip](https://pip.pypa.io/en/stable/installing/) and git. You'll also need to generate a [GitHub personal access token](https://help.github.com/en/articles/creating-a-personal-access-token-for-the-command-line) with full read permissions.

    $ pip install mkdocs mkdocs-git-committers-plugin
    $ git clone https://github.com/nerva-project/nerva-docs.git
    $ cd nerva-docs
    $ git clone https://github.com/nerva-project/nerva-docs-theme.git
    $ MKDOCS_GIT_COMMITTERS_APIKEY=<Your-Access-Token-Here> mkdocs build
The generated HTML files will be placed in the **site** subdirectory.



[nerva-discord-link]: https://discord.gg/ufysfvcFwe
