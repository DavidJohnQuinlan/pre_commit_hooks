## Git pre-commit hooks

### Introduction
Git pre-commits hooks are a very helpful tool which enables a user to identify simple 
issues before submission of code for a code review. Hooks can be configured to run prior to 
every commit to automatically point out issues in code such as missing semicolons, 
trailing whitespace, and debug statements. By pointing these issues out before code 
review, this allows a code reviewer to focus on the architecture of a change while not 
wasting time with trivial style nitpicks.

The `pre-commit` Python package is a very useful package that allows a user to 
configure and define their own pre-commit requirements. It is a multi-language package 
manager for pre-commit hooks. You specify a list of hooks you want and pre-commit 
manages the installation and execution of any hook written in any language before 
every commit.  

The following instructions will detail the installation and initial configuration of 
the pre-commit package and some pre-commit hooks.  

### Installation and configuration
To install the `pre-commit` Python package use the following command in a Python 
environment:

`pip install pre-commit` 

Now that pre-commit has been installed, add it to the repositories `requirements.txt` 
file.

Next, it is necessary to create the `.pre-commit-config.yaml` file. This file defines the 
hooks one wishes to include and the locations of there respective repositories. It 
is also possible to define some initial basic pre-commit hooks in this file too. For 
example some hooks have been added to check for files too large for git or files that 
contain merge conflict strings. For more details on potentially useful hooks see 
`https://pre-commit.com/hooks.html`.

It is also worth noting that pre-commit only runs against staged files. Pre-commit 
will continue to complain when commiting until the required changes have been made 
and changed files subsequently added.

The `isort`, `black` and `flake8` plugin hooks can also be used to ensure that the 
Python code satisfies a basic set of style requirements.
 
#### isort
isort is a Python library which sorts imports alphabetically. It also automatically 
separates libraries into sections sorted by type.

#### black

The Python community rallied behind PEP8 as the guide for what Python code should 
aspire to. PEP8 adds a level of consistency which makes moving from one project to 
another feel natural. However, PEP8 is a style guide not a tool. The consequence of 
focusing on a style guide instead of a tool is that it’s possible to have 
inconsistent code. Thus, placing the burden on the developer to fix any style issues 
in the code.

Black is the uncompromising Python code formatter which solves the above issue by 
automatically formatting your code. By using Black, you agree to cede control over 
very minutiae of hand-formatting. In return, Black gives you speed, determinism, 
and freedom from pycodestyle nagging about formatting. You will save time and mental 
energy for more important matters.

Black makes code reviews faster by producing the smallest diffs possible. Blackened 
code looks the same regardless of the project you are reading. Formatting becomes 
transparent after a while and it is possible to focus on the content instead.

#### flake8
Flake8 is a Python library for checking your code base against coding style (PEP8), 
programming errors (like “library imported but unused” and “Undefined name”) and to 
check cyclomatic complexity.

### Configuration continued
It is possible to define the configurations of `isort` and `flake8` using the 
`setup.cfg` file, however, it is not possible to add `black` configuration to this 
file. Black generally defines its configuration in a file called `pyproject.toml`. 
However, it is possible to define some configuration arguments which are applied on 
installation of Black. In this way there is no need to create another configuration 
file which in my opinion adds further clutter to a repository.

Here is an example of the `setup.cfg` file:

```
# define the isort configurations
[isort]
line_length = 88
ensure_newline_before_comments = True
multi_line_output = 3
include_trailing_comma = True
force_grid_wrap = 0
use_parentheses = True
known_third_party = flask

# define the flake8 configurations
[flake8]
max-line-length = 88
extend-ignore = E203

```    

It can occur that `isort`, `black` and `flake8` configuration conflict each other 
with changes due to one package causing the other to fail. For example, isort helps 
to sort and format imports in your Python code. Black also formats imports, but in 
a different way from isorts defaults which leads to conflicting changes. Similarly, 
Flake8 has a few minor deviations that cause incompatibilities with Black. Both of 
these discrepancies are resolved using the configuration stated in the `setup.cfg` 
file above. For further details on the exact discrepancies see 
`https://black.readthedocs.io/en/stable/compatible_configs.html`.

Now, that all configurations have been defined it is possible to install all pre-commit hooks. 
The following command also ensures that the pre-commit hook will run everytime a 
commit it made to this repository.

`pre-commit install` 

It is usually a good idea to run the hooks against all of the files when adding new 
hooks (usually pre-commit will only run on the changed files during git hooks). To do 
so use the following command: 

`pre-commit run --all-files`

Once all issues have been resolved, any further commits will be automatically checked 
using the pre-commit hooks.

### Conclusion
This repository contains the details required to install and implement pre-commit hooks within a new or existing project.
