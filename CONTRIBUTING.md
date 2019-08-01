# Contributing to Rpi Curious
[heading__title]:
  #contributing-to-rpi-curious
  "&#x2B06; Top of this document"


Thanks for even thinking about it!


The text within this document are not _set in stone_ rules but guidelines, and much like other files maintained by Rpi Curious the following may be edited via a _`Pull Request`_.



------


#### Table of Contents


- [:arrow_up: Top of this document][heading__title]

- [:customs: Code of Conduct][heading__code_of_conduct]

- [How to Contribute][heading__how_to_contribute]

  - [Open Issues][heading__open_issues]
  - [Report Bugs][heading__report_bugs]
  - [Suggest Enhancements][heading__suggest_enhancements]

- [Style Guidelines][heading__style_guidelines]

  - [:shell: Bash][heading__style_guidelines__bash]
  - [:memo: MarkDown][heading__style_guidelines__markdown]

- [Local Development Setup][heading__local_development_setup]

  - [Linux][heading__linux_development_setup]
  - [Windows][heading__windows_development_setup]

- [Git Tips][heading__git_tips]

  - [Commits][heading__git_commit_tips]
  - [Branching][heading__git_branch_tips]

    - [Development][heading__development_git_branches]
    - [Merging][heading__git_merge_tips]

- [Pull Requests][heading__pull_requests]

- [:copyright: License][heading__license]

- [Attribution][heading__attribution]


------


## Code of Conduct
[heading__code_of_conduct]:
  #code-of-conduct
  "&#x1F6C3; A teaser pulled from contributor-covenant Code of Conduct"


```
## Scope

This Code of Conduct applies both within project spaces and in public spaces
when an individual is representing the project or its community. Examples of
representing a project or community include using an official project e-mail
address, posting via an official social media account, or acting as an appointed
representative at an online or offline event. Representation of a project may be
further defined and clarified by project maintainers.

```


Review the [whole thing][branch__current__code_of_conduct] to see what is expected of those that maintain code with Rpi Curious




## How to Contribute
[heading__how_to_contribute]:
  #how-to-contribute
  ""


The goal is, as always, useful code and documentation, though <sub>[![Support][badge__support]][branch__current__support]</sub> is always appreciated. Sharing Repositories maintained by this Organization is an excellent way to contribute if none of the following options are applicable, because the more eyes on a Code Base the more likely it seems that bugs will be found and fixed.


### Open Issues
[heading__open_issues]:
  #open-issues
  ""


Join the <sub>[![Contributors][badge__contributors]][branch__current__network_members]</sub> issuing Pull Requests that close available <sub>[![Open Issues][badge__issues]][branch__current__issues]</sub>. [New Issues][branch__current__issues_new] may be opened for Reporting Bugs and Suggesting Enhancements. However, please search for existing similar Issues first; example search for [`memory-leaks`](https://github.com/rpi-curious/.github/search?q=memory-leaks&type=Issues) from the `.github` repository.

### Report Bugs
[heading__report_bugs]:
  #report-bugs
  ""


The [`bug_report.md`][branch__current__bug_report] Template should when opening a [New Issue][branch__current__issues_new]. Please be detailed and try to include all relevant information within the OP (Original Post). Additionally, if clarifications or more information is requested or discovered, then editing the OP is preferred to adding another post or opening a new issue.


Bugs may also be reported via a Pull Request that suggests fixes, in which case skip opening an Issue and instead use the `:bug:` emoji_word as the first _word_ of the fix commit.


**Example Bug Report Pull Request**


```Bash
git checkout master
git merge --strategy-option theirs --squash dev-master


git commit -F- <<'EOF'
:bug: memory leak `cow_bell(solo_count_down)` when `solo_count_down=<NaN>`



**Fixes**


- `new-script.sh` file, fixes `cow_bell(solo_count_down)` being called before previous solo has ended

- `README.md` file, removes warnings about excessive cow_bell solos causing memory leaks
EOF


git push forked master
```


### Suggest Enhancements
[heading__suggest_enhancements]:
  #suggest-enhancements
  ""


The [`feature_request.md`][branch__current__feature_request] Template may be used when opening a [New Issue][branch__current__issues_new]. Whenever possible provide example/pseudo code along with a detailed description of what needs solved. Or in other-words; napkin-sketches are permitted if it helps readers better understand the scope.


Or for faster consideration and adoption of new code, try adding new features via a Pull Request


**Example Enhancement Pull Request**


```Bash
git checkout gh-pages
git merge --strategy-option theirs --squash dev-gh-pages


git commit -F- <<'EOF'
:apple: Cow Bell solos supported on Mac OS



**Additions**


`new-script.sh` file;


- feature detection for Mac OS, uses default media player for solos

- feature detection for MS, however requires Bash sub-system to be installed
EOF


git push forked gh-pages
```

___


## Style Guidelines
[heading__style_guidelines]:
  #style-guidelines
  ""


No-one _should_ get offended if a new line is forgotten or similar, but please do **not** break anything when issuing Pull Requests.


- Code that operates as intended is as important as documentation that is comprehensible, so do **not** sacrifice readability for anything

- In most cases two (**`2`**) should be used between sections, one (**`1`**) between sub-sections, and three (**`3`**) between headers and footers or _boilerplate_ sections

- URLs may _break_ column width limits

- Tabs shall be a sequence of four spaces (**`    `**), generally no literal tabs (`\t`) will be permitted within code or documentation

- Project, File, and Directory names _should_ be lowercase, with hyphens (**`-`**) in place of spaces (**` `**) except where GitHub requires otherwise

- Documentation should be no more than `1024` lines per file

- Code specific (`.sh`) files should aim for less than `512` lines of actionable code, and _doc-strings_/comments should not exceed `20%` of total lines within such files

- _Unix-ish_ new-lines (**`\n`**) are to be used within all files, Pull Request using other line-breaks will be rejected until corrected


### Bash Style Guidelines
[heading__style_guidelines__bash]:
  #bash-style-guidelines
  "&#x1F41A; Style Guidelines for Bash"


- Lines should not exceed `120` columns wide for code and no more than `80` columns wide for comment blocks

- Comments in most cases should precede code

- Variable and function names should be descriptive and underscores (**`_`**) in place of spaces (**` `**) or hyphens (**`-`**) between words, please do not issue Pull Requests with _`camelCased`_ names

- Use `declare -g` sparingly, and `local` variable assignments where necessary

- Return _something_ from Functions even if that is a boolean status of success or failure

- Properly handle errors and/or allow errors to _bubble up_, _ask permission_ when necessary, and fail fast

- `continue` past non-matches within loops to avoid _over-nesting_ of conditional logic


**`shared-functions/modules/trap-failure/failure.sh`**


```Bash
#!/usr/bin/env bash


## Outputs Front-Mater formatted failures for functions not returning 0
## Use the following line after sourcing this file to set failure trap
##    trap 'failure "LINENO" "BASH_LINENO" "${BASH_COMMAND}" "${?}"' ERR
failure(){
    local -n _lineno="${1:-LINENO}"
    local -n _bash_lineno="${2:-BASH_LINENO}"
    local _last_command="${3:-${BASH_COMMAND}}"
    local _code="${4:-0}"

    ## Workaround for read EOF combo tripping traps
    if ! ((_code)); then
        return "${_code}"
    fi

    local _last_command_height="$(wc -l <<<"${_last_command}")"

    local -a _output_array=()
    _output_array+=(
        '---'
        "lines_history: [${_lineno} ${_bash_lineno[*]}]"
        "function_trace: [${FUNCNAME[*]}]"
        "exit_code: ${_code}"
    )

    if [[ "${#BASH_SOURCE[@]}" -gt '1' ]]; then
        _output_array+=('source_trace:')
        for _item in "${BASH_SOURCE[@]}"; do
            _output_array+=("  - ${_item}")
        done
    else
        _output_array+=("source_trace: [${BASH_SOURCE[*]}]")
    fi

    if [[ "${_last_command_height}" -gt '1' ]]; then
        _output_array+=(
            'last_command: ->'
            "${_last_command}"
        )
    else
        _output_array+=("last_command: ${_last_command}")
    fi

    _output_array+=('---')
    printf '%s\n' "${_output_array[@]}" >&2
    exit ${_code}
}
```


### MarkDown Style Guidelines
[heading__style_guidelines__markdown]:
  #markdown-style-guidelines
  "&#x1F4DD; Style Guidelines for MarkDown"


- There is no set column width limits for MarkDown files, but do not get carried away because the focus should be on getting readers _up to speed_

- External links are permitted but try to keep it to a minimum; instead refer to built-in `man` and/or `help` documentation wherever possible

- Relative links to other files or locations within documentation is preferred; except for _cross-branch_ linking in which case absolute links are preferred

  - Links be should organized into a Links Section bellow the Content, after three (**`3`**) blank lines
  - Link Names should use two underscores between base location and file or title

- Lists should have one blank line between first layer of items

  - Inner lists should be tabbed in by two (**`2`**) spaces and have no blank lines between items of the same level
  - Nesting lists beyond one layer is discouraged and information should be restructured when it becomes tempting to do otherwise

- Provide a _`Table of Contents`_ section when headings become numerous and use three underscores (**`___`**) as Dividers between main sections

  - Each heading should have an internal MarkDown _tag_, see following example, used for linking within the document
  - Only one _level one_ heading (lines prefixed with a single **`#`** one hash-sign) may be included in a MarkDown document and only when no _`FrontMatter`_ defines a **`title:`** property
  - Main Sections should have a _level two_ heading (prefixed by **`##`** two hash-signs)
  - Sub-Sections should have a _level three_ heading, and nesting beyond this is discouraged

- Code Blocks should have the related language defined as proper nouns, eg. `Bash` not `bash`

  - Formatting within code blocks should follow related guidelines for that language
  - Code blocks should not exceed **`120`** lines in length
  - Use links to files instead of copying files into Code Blocks
  - Title code blocks with bold back-ticks and example path for re-use elsewhere within documentation...


```MarkDown
**`file-name.ext`**
```


- Two blank lines should go between Sections, Sub-Sections, Dividers, Code Blocks, and List Blocks.

- Three blank lines should go between Description, and between Content and Links Section


**`readme.md`**


```MarkDown
# Title of Document


Short description about content of document, project, and/or code



------


#### Table of Contents


- [First][heading__first_section]

- [Second][heading__second_section]

  - [Bash Time][heading__bash_time]
  - [Bash Variables][heading__bash_variables]

- [End][heading__end_section]


------


## First Section
[heading__first_section]:
  #first-section
  "Link hover-text for first section"


A thing or two about `git`...


\```Bash
_name='S0AndS0'
_repo='.github'

git clone git@github.com:${_name}/${_repo}.git
\```


Maybe a table with some columns to organize something...


| Column One | Column Two |
|------------|------------|
| cell       | cell       |
| cell       | cell       |


___


## Second Section
[heading__second_section]:
  #second-section
  "Link hover-text for second section"


Some notes about _`Bash`_


### Bash Time
[heading__bash_time]:
  #bash-time
  "Link hover-text for Bash time"


**[`time-stamp-date.sh`][branch__current__another_file]**


\```Bash
time_stamp_date() {
  local _date="${1:-$(date)}"
  printf '%s\n' "$(date --date="${_date}" +'%Y%m%d')"
}
\```


### Bash Variables
[heading__bash_variables]:
  #bash-variables
  "Link hover-text for Bash variables"


Interactive console examples...

\```Bash
_now_time_stamp="$(time_stamp_date)"
printf '%s\n' "${_now_time_stamp}"
#> 20191125


_past_time_stamp="$(time_stamp_date 'July 01, 1970')"
printf '%s\n' "${_past_time_stamp}"
#> 19700701
\```


___


## End Section
[heading__end_section]:
  #end-section
  "Link hover-text for end section"


In summation this is the general outline of MarkDown formatting.


Check [`gh-pages`][branch__gh_pages] branch for example usage and documentation.


See [Somewhere Else][example__somewhere_else] for more details on something else.



[branch__current__another_file]: time-stamp-date.sh


[branch__gh_pages]: https://github.com/bash-utilizes/project-name/tree/gh-pages


[example__somewhere_else]: https://example.com/somewhere-else.html
```


> Note, any prefixed back-slashes (`\` ) should be removed from above example



___


## Local Development Setup
[heading__local_development_setup]:
  #local-development-setup
  ""


For repositories that include a `_config.yml` file within a `gh-pages` branch then Jekyll is required for building documentation, see the [Jekyll Admin][jekyll_admin] for setup and automation scripts built to make setup tasks a little swifter. Otherwise most projects of this Organization only require a Bash shell that is reasonably up-to date.


### Linux Development Setup
[heading__linux_development_setup]:
  #linux-development-setup
  ""

> The following steps and variable usage may also work on Mac, and may be Windows **if** a Bash shell is available.


**Shared Variables**


```Bash
_git_name='your-name'
_organization='rpi-curious'
_repository='project-name'


_https_origin_url="https://github.com/${_organization}/${_repository}.git"
_git_origin_url="git@github.com:${_organization}/${_repository}.git"
_https_fork_url="https://github.com/${_git_name}/${_repository}.git"
_git_fork_url="git@github.com:${_git_name}/${_repository}.git"
```


> The above Bash variables will be referenced within following sub-sections, modify the values to suite the Forked Repository.


Setup remotes via one of the following;


1. Make a directory path for Git sources and change directories

2. Clone fork, checkout `gh-pages` or `example` branch, and setup origin tracking

3. Setup tracking of fork HTTPS URL tracking from perspective of project root

4. Setup tracking of fork Git URL tracking from perspective of submodule root


```bash
mkdir -vp "${HOME}/github/${_git_name}"
cd "${HOME}/github/${_git_name}"


git clone --origin forked "${_git_fork_url}"
cd "${_repository}"
git checkout gh-pages
git remote add origin "${_git_origin_url}"


git config --file=.gitmodules submodule.browser-storage.url "${_https_fork_url}"
git submodule sync
git submodule update --init --recursive --remote


cd "modules/${_repository}"
git remote add forked "${_git_fork_url}"
git fetch forked
git branch --set-upstream-to=forked/master
```


_`git push forked master`_ _should_ push to the forked repository URL, and _`git fetch origin master`_ will download any updates from this Organization. If any updates where downloaded then be sure to merge before issuing a Pull Request.


### Windows Development Setup
[heading__windows_development_setup]:
  #windows-development-setup
  ""

**Batch Variables**


```Batch
set _git_name='your-name'
set _organization='rpi-curious'
set _repository='project-name'

set _https_origin_url="https://github.com/%_organization/%_repository.git"
set _git_origin_url="git@github.com:%_organization/%_repository.git"
set _https_fork_url="https://github.com/${_git_name}/${_repository}.git"
set _git_fork_url="git@github.com:%_git_name/%_repository.git"
```

**Batch Git Commands**


```Batch
setlocal enableextensions enabledelayedexpansion


md %HOMEDRIVE%%HOMEPATH%\github\%_git_name
cd %HOMEDRIVE%%HOMEPATH%\github\%_git_name


git clone --origin forked %_git_fork_url
cd %_repository
git checkout gh-pages
git remote add origin %_git_origin_url


git config --file=.gitmodules submodule.browser-storage.url %_https_fork_url
git submodule sync
git submodule update --init --recursive --remote

cd modules\%_repository
git remote add forked %_git_fork_url
git fetch forked
git branch --set-upstream-to=forked/master

```

> Pull Requests are most welcome for correcting anything that might be erroneous.

___


## Git Tips
[heading__git_tips]:
  #git-tips
  ""


This will not be an in-depth or exhaustive guide on `git` usage, as the preexisting documentation available via commands such as _`git help`_ and _`git help submodule`_ are thorough.


### Git Commit Tips
[heading__git_commit_tips]:
  #git-commit-tips
  ""


- First line should not exceed `74` columns wide and punctuation such as apostrophes, quotes, and periods (`"'."`) should be avoided

- First line should be separated from message content by three blank lines

- While not required the following emoji_word may be used as the first _word_ of commit messages

  - :tada:             `:tada:` for `Initial Commit` of repository, **not** to be used when re-naming files
  - :art:              `:art:` for format and/or structure related changes
  - :shell: `:shell:` for changes to Bash files.
  - :memo: `:memo:` for changes to MarkDown files.
  - :fire:             `:fire:` for deletion of files, code, or documentation
  - :hankey:           `:hankey:` please avoid needing to use as it's for when moving files or content between branches
  - :dizzy:            `:dizzy:` when re-naming or moving files within a branch, it'll happen for newer projects but need for use is to be avoided past version **`0.0.5`**

  - :bug:              `:bug:` for _stomping_ bugs in general
  - :smoking:          `:smoking:` for resource bug fixes, eg. memory leaks, recursion limits, CPU load
  - :facepunch:        `:facepunch:` when _blaming_ one's self and new commit is to fix bug from recently past commit
  - :do_not_litter:    `:do_not_litter:` when _blaming_ another's recent changes for requiring new committed bug fix
  - :lock:             `:lock:` for security related fixes

  - :penguin:          `:penguin:` for fixing or improving Linux performance or compatibility
  - :apple:            `:apple:` for fixing or improving Apple/Mac performance or compatibility
  - :checkered_flag:   `:checkered_flag:` for fixing or improving Windows/MS performance or compatibility

  - :arrow_up:         `:arrow_up:` for tracking upgraded dependencies
  - :arrow_down:       `:arrow_down:` for tracking downgraded dependencies
  - :bookmark:         `:bookmark:` for Tagging Releases and Request For Comments (RFC)

  - :white_check_mark: `:white_check_mark:` for adding tests
  - :green_heart:      `:green_heart:` when fixing Continuous Integration builds

  - :ship:             `:ship:` when opening a Pull Request
  - :stars:            `:stars:` for accepting a Pull Request
  - :no_entry:         `:no_entry:` for rejecting a Pull Request


Additional notes should follow [Markdown Style Guidelines][heading__style_guidelines__markdown]; except for headings as _`#`s_ are considered comments by default and thus ignored by many `commit` message handlers, see following example for other formatting differences


```Bash
git add README.md
git commit -F- <<'EOF'
:memo: Adds more readme content and spelling corrections



**Additions**
Links where tested locally and new content should explain things better.


**Corrections**
Other than spelling there where a few confusing spots that where also reworded.
EOF
```


### Git Branch Tips
[heading__git_branch_tips]:
  #git-branch-tips
  ""


There may be _`orphan`_ branches utilized by rpi-curious for separating Code to be included in other projects from Documentation and Usage Examples. Non-orphaned development branches are encouraged when adding features or fixing bugs.


**`master`** branch generally contain;


- _`lib`_ or _`shared-functions`_ directory, should **only** be used for files that directly support the _`script-name.sh`_ file, otherwise please split out reusable code to separate repositories for including as submodules within the `gh-pages` branch

- `.git/` directory, required for version tracking and logging changes

- _`.github/README.md`_ file, should be a _quick start_ documenting installation and/or usage

- _`script-name.sh`_ file, any dependencies should be listed within the _`.gitmodules`_ file under the **`gh-pages`** branch


**`gh-pages`** branch (sometimes `example` branch) may contain;


- `modules` directory, that contains a submodule subdirectory tracking the `master` branch, eg. `modules/trap-failure`

- `.gitmodules` file, used by Git to version track submodules

- `.travis.yml` file, used by Travis CI for public automated tests of code

- `.github/README.md` file, should be a _quick start_ on development setup for fixing bugs or adding features via Pull Requests

- `example_usage.sh` file, should demonstrate how code from the `master` branch is intended to be used


All branches should contain a `LICENSE` file, each file should make reference to the license in use, and Pull Requests may be opened only if shared under the same license for a given file and/or repository.


Depending upon language(s) utilized by a given repository the above file structure may change slightly, however, each branch should be kept _spartan_!


#### Development Git Branches
[heading__development_git_branches]:
  #development-git-branches
  ""


Development branches are excellent for privately tracking series of changes for new features or especially pervasive bugs. Merging with a _`squash`_ commit back to one of the _main line_ branches prior to publicly pushing to a fork is encouraged, however, please try to be _targeted_ as to what each committed change pertains to.


**Example `dev-master` branch initialization**


```bash
cd "${HOME}/github/${_organization}/new-project"


git checkout master
git checkout -b dev-master
```


Commit changes to `dev-master`, then after tests have passed preform a `squash` merge of `dev-master` from the `master` branch


```Bash
git checkout master
git merge --strategy-option theirs --squash dev-master

git commit -F- <<'EOF'
:bug: Fixes volume not being set to _`11`_ for solos



**Edits**
`cow_bell.sh` file, sets volume to max when `cow_bell()` solo starts
EOF


git push forked master
```


#### Git Merge Tips
[heading__git_merge_tips]:
  #git-merge-tips
  ""


**Never** `git merge master` from the `gh-pages` branch, and definitely do **not** `git merge gh-pages` when the `master` branch is checked out; orphaned branches should only be merged to and from their respective development (**_`dev-`_**) prefixed branches.


Squash merging is preferred for targeted edits, eg...


```bash
git checkout master
git merge --strategy-option theirs --squash dev-master


git commit -F- <<'EOF'
:shell: Asking permission from checkboxes before modifying state



**Changes**
`new-script.js` file, `uncheck_all()` continues on unchecked boxes and `check_all()` continues on checked boxes
EOF


git push forked master
```


Also be wary of _`rm`_ vs _`git rm`_ and _`mv`_ vs _`git mv`_ commands, when merging from a development branch back to one of the _mainline_ branches the _non-`git`_ wrapped commands will **not** update state between branches, and Pull Requests that confuse version management will be rejected.


___


## Pull Requests
[heading__pull_requests]:
  #pull-requests
  ""


Issuing Pull Requests to repositories maintained by this Organization will only be considered **if** shared under the same licensing defined under [License](#license) section of this document. Please use any relevant examples from the [`pull_request.md`][branch__current__pull_request] Template and adherer to the Style Guidelines for [Git Commits](#git-commit-tips)



___


## License
[heading__license]:
  #license
  "&#x2696; Legal side of Open Source"


```
Miscellaneous tips and scripts for Raspberry Pi devices.
Copyright (C) <year> S0AndS0

This program is free software: you can redistribute it and/or modify
it under the terms of the GNU Affero General Public License as published
by the Free Software Foundation, version 3 of the License.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU Affero General Public License for more details.

You should have received a copy of the GNU Affero General Public License
along with this program.  If not, see <https://www.gnu.org/licenses/>.

```


For further details review full length version of [AGPL-3.0][branch__current__license] License.


___


## Attribution
[heading__attribution]:
  #attribution
  ""


Portions of this document, such as emoji_word usage, where inspired by [_`contributing.md`_][atom__contributing] guidelines from the Atom IDE development team.

Templating of this and other documents within the repository where compiled with [Mustache][mustache_js] _powers_



[branch__current__issues]:
  issues
  "Please search for existing issues that may be added to or closed"

[branch__current__issues_new]:
  issues/new
  "Select the appropriate template when available"

[branch__current__network_members]:
  network/members
  "Perhaps you too may be counted amount those that have Forked repositories maintain by Rpi Curious"

[branch__current__support]:
  SUPPORT.md
  "Best avenues for seeking support of Rpi Curious"


[branch__current__code_of_conduct]:
  CODE_OF_CONDUCT.md
  "Please do **not** encourage new guidelines to be added"

[branch__current__bug_report]:
  ISSUE_TEMPLATE/bug_report.md
  "Create a report to help us improve"

[branch__current__feature_request]:
  ISSUE_TEMPLATE/feature_request.md
  "Suggest an idea for this project"

[branch__current__pull_request]:
  pull_request_template.md
  "Template for general Pull Request"

[branch__current__pull_request__bug_fix]:
  PULL_REQUEST_TEMPLATE/bug_fix.md
  "Template for Pull Request that fixes a bug"

[branch__current__pull_request__feature_addition]:
  PULL_REQUEST_TEMPLATE/feature_addition.md
  "Template for Pull Request that adds a feature"

[branch__current__readme]:
  README.md
  "Highlights various resources this repository contains"


[branch__current__license]:
  /LICENSE
  "&#x2696; Full length version of AGPL-3.0 License"


[branch__current__security]:
  /SECURITY.md
  "Best practices for reporting issues of a security related nature"


[badge__issues]: https://img.shields.io/github/issues/rpi-curious/.github.svg

[badge__contributors]: https://img.shields.io/github/forks/rpi-curious/.github.svg?color=005571&label=Contributors

[badge__support]: https://img.shields.io/badge/&hearts;-Support-lightgray.svg?labelColor=success&color=gray


[atom__contributing]: https://github.com/atom/atom/blob/master/CONTRIBUTING.md

[mustache_js]: https://github.com/janl/mustache.js

[jekyll_admin]: https://github.com/S0AndS0/Jekyll_Admin
