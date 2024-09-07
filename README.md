<div id="top" align="center">
<h1>TOC Generator</h1>

<p>`GitHub Actions` to generate TOC (Table of Contents)</p>

[![MIT License](https://img.shields.io/badge/License-MIT-green.svg)](https://choosealicense.com/licenses/mit/)
[![WyriHaximus](https://img.shields.io/badge/fork_from-WyriHaximus-black?logo=github)](https://github.com/WyriHaximus/toc-generator)

![GitHub Created At](https://img.shields.io/github/created-at/Zheng-Bote/gha-toc-generator)

</div>

## Brief

<p>This is a `GitHub Actions` to generate TOC (Table of Contents),  
which executes [DocToc](https://github.com/thlorenz/doctoc) and commits if changed.</p>

<hr>

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
<details>
<summary>Details</summary>

- [Installation](#installation)
- [Screenshot](#screenshot)
- [Options](#options)
  - [Specify options individually](#specify-options-individually)
- [Action event details](#action-event-details)
  - [Target event](#target-event)
  - [Conditions](#conditions)
- [Addition](#addition)
  - [GITHUB_TOKEN](#github_token)
  - [Create PullRequest](#create-pullrequest)
  - [Context variables](#context-variables)
  - [Context PR variables](#context-pr-variables)
- [Configuration Examples](#configuration-examples)
  - [Example 1](#example-1)
  - [Example 2](#example-2)
  - [Example 3](#example-3)
- [Author](#author)

</details>
<!-- END doctoc generated TOC please keep comment here to allow auto update -->

<hr>

## Description

![GHA](https://img.shields.io/badge/Github-Action-black?logo=githubactions)
![Node](https://img.shields.io/badge/Node-20-blue?logo=tsnode)

> [!NOTE]
> @Zheng-Bote: Updated to Node.js v20

<p align="right">(<a href="#top">back to top</a>)</p>

# Status

> [!CAUTION]
> Does **not** work with GHES.

<p align="right">(<a href="#top">back to top</a>)</p>

## Installation

1. Specify location of TOC (option)  
   e.g. `README.md`
   ```markdown
   <!-- START doctoc -->
   <!-- END doctoc -->
   ```
   [detail](https://github.com/thlorenz/doctoc#specifying-location-of-toc)
   
2. Setup workflow  
    e.g. `.github/workflows/repo-create_doctoc.yml`
   
```yaml
   name: Repo - create TOC of README
   # description: https://github.com/technote-space/toc-generator
   # README.md:
   # <!-- START doctoc -->
   # <!-- END doctoc -->

   run-name: create README table of contents by ${{ github.actor }}

   on:
     workflow_dispatch:
     push:
     paths: - './README.md'
    
   permissions:
     contents: write
    
   jobs:
     generateTOC:
       name: TOC Generator
       runs-on: ubuntu-latest
       steps:
         - uses: Zheng-Bote/gha-toc-generator@v4.3.4
           with:
             GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
             TARGET_PATHS: ./README.md
             FOLDING: false
````


<p align="right">(<a href="#top">back to top</a>)</p>

## Parameters

<!-- only for actions repo -->

### Inputs

### Outputs


## Screenshot

![behavior](https://raw.githubusercontent.com/technote-space/toc-generator/images/screenshot.gif)

## Options

| name                      | description                                                                                                              | default                                                   | e.g.                           |
| :------------------------ | :----------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------- | :----------------------------- |
| TARGET_PATHS              | Target file path. (Comma separated, [Detail](https://github.com/thlorenz/doctoc#adding-toc-to-individual-files))         | `README*.md`                                              | `README*.md,CHANGELOG.md`, `.` |
| TOC_TITLE                 | TOC Title                                                                                                                | `**Table of Contents**`                                   | `''`                           |
| MAX_HEADER_LEVEL          | Maximum heading level. ([Detail](https://github.com/thlorenz/doctoc#specifying-a-maximum-heading-level-for-toc-entries)) |                                                           | `3`                            |
| CUSTOM_MODE               | Whether it is custom mode([Generated Example](samples/README.horizontal.md))                                             | `false`                                                   | `true`                         |
| CUSTOM_TEMPLATE           | Custom template for custom mode                                                                                          | `<p align="center">${ITEMS}</p>`                          |                                |
| ITEM_TEMPLATE             | Item template for custom mode                                                                                            | `<a href="${LINK}">${TEXT}</a>`                           |                                |
| SEPARATOR                 | Separator for custom mode                                                                                                | <code>\<span>&#124;\</span></code>                        |                                |
| FOLDING                   | Whether to make TOC foldable                                                                                             | `false`                                                   | `true`                         |
| COMMIT_MESSAGE            | Commit message                                                                                                           | `chore(docs): update TOC`                                 | `docs: update TOC`             |
| COMMIT_NAME               | Git commit name                                                                                                          | `${github.actor}`                                         |                                |
| COMMIT_EMAIL              | Git commit email                                                                                                         | `${github.actor}@users.noreply.github.com`                |                                |
| CREATE_PR                 | Whether to create PullRequest                                                                                            | `false`                                                   | `true`                         |
| CHECK_ONLY_DEFAULT_BRANCH | Whether to check only default branch                                                                                     | `false`                                                   | `true`                         |
| PR_BRANCH_PREFIX          | PullRequest branch prefix                                                                                                | `toc-generator/`                                          |                                |
| PR_BRANCH_NAME            | PullRequest branch name<br>[Context variables](#context-variables)                                                       | `update-toc-${PR_ID}`                                     | `toc-${PR_NUMBER}`             |
| PR_TITLE                  | PullRequest title<br>[Context variables](#context-variables)                                                             | `chore(docs): update TOC (${PR_MERGE_REF})`               | `docs: update TOC`             |
| PR_BODY                   | PullRequest body<br>[Context PR variables](#context-pr-variables)                                                        | [action.yml](action.yml)                                  |                                |
| PR_COMMENT_BODY           | PullRequest body for comment<br>[Context PR variables](#context-pr-variables)                                            | [action.yml](action.yml)                                  |                                |
| PR_CLOSE_MESSAGE          | Message body when closing PullRequest                                                                                    | `This PR has been closed because it is no longer needed.` |                                |
| TARGET_BRANCH_PREFIX      | Filter by branch name                                                                                                    |                                                           | `release/`                     |
| INCLUDE_LABELS            | Labels used to check if the PullRequest has it                                                                           |                                                           | `Label1, Label2`               |
| OPENING_COMMENT           | Opening comment (for other than DocToc)                                                                                  | `<!-- toc `                                               |                                |
| CLOSING_COMMENT           | Closing comment (for other than DocToc)                                                                                  | `<!-- tocstop `                                           |                                |
| SKIP_COMMENT              | Change skip comment (default: `<!-- DOCTOC SKIP `)                                                                       |                                                           | `<!-- toc skip `               |
| GITHUB_TOKEN              | Access token                                                                                                             | `${{github.token}}`                                       | `${{secrets.ACCESS_TOKEN}}`    |
| SIGNOFF                   | Add `Signed-off-by` line                                                                                                 |                                                           | `true`                         |

<p align="right">(<a href="#top">back to top</a>)</p>

### Specify options individually

The options used for [doctoc](https://github.com/technote-space/doctoc#example) can be commented to specify values.
If you want to generate multiple TOCs with different settings, specify the values individually as follows.

e.g.

```markdown
<!-- START doctoc -->
<!-- param::isNotitle::true:: -->
<!-- param::isCustomMode::true:: -->

<!-- END doctoc -->

...
````

<p align="right">(<a href="#top">back to top</a>)</p>

## Action event details

### Target event

| eventName: action                                                  | condition                 |
| :----------------------------------------------------------------- | :------------------------ |
| push: \*                                                           | [condition1](#condition1) |
| pull_request: \[opened, synchronize, reopened, labeled, unlabeled] | [condition2](#condition2) |
| pull_request: \[closed]                                            |                           |
| schedule, repository_dispatch, workflow_dispatch                   |                           |

- The following activity types must be explicitly specified ([detail](https://help.github.com/en/github/automating-your-workflow-with-github-actions/events-that-trigger-workflows#pull-request-event-pull_request))
  - `labeled`, `unlabeled`, `closed`

<p align="right">(<a href="#top">back to top</a>)</p>

### Conditions

#### condition1

- push to branch (not tag)
  - branch name ([`TARGET_BRANCH_PREFIX`](#options))

#### condition2

- specified labels included? ([`INCLUDE_LABELS`](#options))
- branch name ([`TARGET_BRANCH_PREFIX`](#options))

<p align="right">(<a href="#top">back to top</a>)</p>

## Addition

### GITHUB_TOKEN

The `GITHUB_TOKEN` that is provided as a part of `GitHub Actions` doesn't have authorization to create any successive events.  
So it won't spawn actions which triggered by push.

This can be a problem if you have branch protection configured.

If you want to trigger actions, use a personal access token instead.

1. Generate a [personal access token](https://help.github.com/en/articles/creating-a-personal-access-token-for-the-command-line) with the public_repo or repo scope.  
   (repo is required for private repositories).
1. [Save as ACCESS_TOKEN](https://help.github.com/en/actions/configuring-and-managing-workflows/creating-and-storing-encrypted-secrets)
1. Add input to use `ACCESS_TOKEN` instead of `GITHUB_TOKEN`.  
   e.g. `.github/workflows/toc.yml`
   ```yaml
   on: push
   name: TOC Generator
   jobs:
     generateTOC:
       name: TOC Generator
       runs-on: ubuntu-latest
       steps:
         - uses: technote-space/toc-generator@v4
           with:
             GITHUB_TOKEN: ${{ secrets.ACCESS_TOKEN }}
   ```

<p align="right">(<a href="#top">back to top</a>)</p>

### Create PullRequest

If `CREATE_PR` is set to `true`, a PullRequest is created.

```yaml
on: pull_request
name: TOC Generator
jobs:
  generateTOC:
    name: TOC Generator
    runs-on: ubuntu-latest
    steps:
      - uses: technote-space/toc-generator@v4
        with:
          CREATE_PR: true
```

![create pr](https://raw.githubusercontent.com/technote-space/toc-generator/images/create_pr.png)

If the `closed` activity type is set, this action closes the PR when it is no longer needed.

```yaml
on:
  pull_request:
    types: [opened, synchronize, reopened, closed]
name: TOC Generator
jobs:
  generateTOC:
    name: TOC Generator
    runs-on: ubuntu-latest
    steps:
      - uses: technote-space/toc-generator@v4
```

<p align="right">(<a href="#top">back to top</a>)</p>

### Context variables

| name          | description                                                         |
| :------------ | :------------------------------------------------------------------ |
| PR_NUMBER     | pull_request.number (e.g. `11`)                                     |
| PR_NUMBER_REF | `#${pull_request.number}` (e.g. `#11`)                              |
| PR_ID         | pull_request.id (e.g. `21031067`)                                   |
| PR_HEAD_REF   | pull_request.head.ref (e.g. `change`)                               |
| PR_BASE_REF   | pull_request.base.ref (e.g. `main`)                                 |
| PR_MERGE_REF  | pull_request.base.ref (e.g. `change -> main`)                       |
| PR_TITLE      | pull_request.title (e.g. `update the README with new information.`) |

[Payload example](https://developer.github.com/v3/activity/events/types/#webhook-payload-example-28)

### Context PR variables

- [Context variables](#context-variables)

| name            | description            |
| :-------------- | :--------------------- |
| PR_LINK         | Link to PR             |
| COMMANDS_OUTPUT | Result of TOC command  |
| FILES_SUMMARY   | e.g. `Changed 2 files` |
| FILES           | Changed file list      |

<p align="right">(<a href="#top">back to top</a>)</p>

## Configuration Examples

### Example 1

Execute actions at push without limiting the branch and commit directly

```yaml
on: push
name: TOC Generator
jobs:
  generateTOC:
    name: TOC Generator
    runs-on: ubuntu-latest
    steps:
      - uses: technote-space/toc-generator@v4
```

### Example 2

Create or update a Pull Request by executing actions on a Pull Request update only for branches starting with `release/`.

```yaml
on:
  pull_request:
    types: [opened, synchronize, reopened, closed]
name: TOC Generator
jobs:
  generateTOC:
    name: TOC Generator
    runs-on: ubuntu-latest
    steps:
      - uses: technote-space/toc-generator@v4
        with:
          CREATE_PR: true
          TARGET_BRANCH_PREFIX: release/
```

### Example 3

Execute actions in the schedule for the default branch only and commit directly.  
（Using the Token created for the launch of other workflows）

```yaml
on:
  schedule:
    - cron: "0 23 * * *"
name: TOC Generator
jobs:
  generateTOC:
    name: TOC Generator
    runs-on: ubuntu-latest
    steps:
      - uses: technote-space/toc-generator@v4
        with:
          GITHUB_TOKEN: ${{ secrets.ACCESS_TOKEN }}
          CHECK_ONLY_DEFAULT_BRANCH: true
```

<p align="right">(<a href="#top">back to top</a>)</p>

### folder structure

<!-- readme-tree start -->
```
.
├── .commitlintrc
├── .editorconfig
├── .eslintrc
├── .github
│   ├── CODEOWNERS
│   ├── CODE_OF_CONDUCT.md
│   ├── CONTRIBUTING.md
│   ├── FUNDING.yml
│   ├── ISSUE_TEMPLATE
│   │   ├── bug_report.md
│   │   └── feature_request.md
│   ├── card-labeler.yml
│   ├── config.yml
│   ├── labeler.yml
│   ├── no-response.yml
│   ├── pr-labeler.yml
│   ├── pull_request_template.md
│   ├── release-drafter.yml
│   ├── stale.yml
│   ├── workflow-details.json
│   ├── workflow-settings.json
│   └── workflows
│       ├── add-release-tag.yml
│       ├── add-test-tag.yml
│       ├── check-warnings.yml
│       ├── ci.yml
│       ├── issue-opened.yml
│       ├── pr-opened.yml
│       ├── pr-updated.yml
│       ├── project-card-moved.yml
│       ├── repo-actions_docu.yml
│       ├── repo-create_doctoc.yml
│       ├── repo-create_tree_readme.yml
│       ├── sync-workflows.yml
│       ├── toc.yml
│       └── update-dependencies.yml
├── .gitignore
├── .husky
│   ├── .gitignore
│   ├── commit-msg
│   └── pre-commit
├── .lintstagedrc
├── .releasegarc
├── LICENSE
├── README.ja.md
├── README.md
├── _config.yml
├── action.yml
├── package-lock.json
├── package.json
├── rollup.config.mjs
├── samples
│   ├── README.horizontal.md
│   └── README.not-folding.md
├── src
│   ├── constant.ts
│   ├── fixtures
│   │   ├── doctoc
│   │   │   ├── README.create1.md
│   │   │   ├── README.create2.md
│   │   │   ├── README.horizontal.md
│   │   │   ├── README.not.update.md
│   │   │   ├── README.params.md
│   │   │   ├── README.skip.md
│   │   │   ├── README.toc-me.md
│   │   │   ├── README.update.md
│   │   │   └── expected
│   │   │       ├── README.create1.md
│   │   │       ├── README.create2.md
│   │   │       ├── README.horizontal1.md
│   │   │       ├── README.horizontal2.md
│   │   │       ├── README.params.md
│   │   │       ├── README.toc-me.md
│   │   │       ├── README.update.md
│   │   │       ├── README.update.options.md
│   │   │       └── README.update.wrap.md
│   │   ├── issues.comment.create.json
│   │   ├── pulls.get.json
│   │   ├── pulls.list.json
│   │   ├── pulls.update.json
│   │   ├── repos.get.json
│   │   ├── repos.git.blobs.json
│   │   ├── repos.git.commits.get.json
│   │   ├── repos.git.commits.json
│   │   ├── repos.git.refs.create.json
│   │   ├── repos.git.refs.update.json
│   │   ├── repos.git.trees.json
│   │   └── test.md
│   ├── main.ts
│   ├── setup.ts
│   └── utils
│       ├── doctoc.test.ts
│       ├── doctoc.ts
│       ├── misc.test.ts
│       ├── misc.ts
│       ├── process.test.ts
│       └── transform.ts
├── tree.bak
├── tsconfig.json
├── vite.config.ts
└── yarn.lock

10 directories, 91 files
```
<!-- readme-tree end -->

<p align="right">(<a href="#top">back to top</a>)</p>

### License

#### MIT

MIT License

Copyright (c) \[year] \[fullname]

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

        https://choosealicense.com/licenses/mit/

<p align="right">(<a href="#top">back to top</a>)</p>

## Author

[GitHub (Technote)](https://github.com/technote-space)  
[Blog](https://technote.space)

## Code Contributors

![Contributors](https://img.shields.io/github/contributors/technote-space/toc-generator?color=dark-green)

[![Zheng Robert](https://img.shields.io/badge/Github-Zheng_Robert-black?logo=github)](https://www.github.com/Zheng-Bote)

<hr>

:vulcan_salute:

<p align="right">(<a href="#top">back to top</a>)</p>
