# Temporal documentation

Hello, World!

Welcome to Temporal's documentation repository!

**Table of contents**

- [What is the goal of this README?](#what-is-the-goal-of-this-readme)
  - [What is the “Temporal Platform information corpus”?](#what-is-the-temporal-platform-information-corpus)
- [What is in this repository?](#what-is-in-this-repository)
  - [/docs-src information nodes](#docs-src-information-nodes)
  - [/docs generated files for Docusaurus](#docs-generated-files-for-docusaurus)
  - [/assembly Assembly Workflow](#assembly-assembly-workflow)
  - [Snipsync code synchronization tooling](#snipsync-code-synchronization-tooling)
- [How to get approval to create a pull request](#how-to-get-approval-to-create-a-pull-request)
- [How to fix a typo](#how-to-fix-a-typo)
- [How to make changes to this repository](#how-to-make-changes-to-this-repository)
  - [How to install the dependencies for this repo](#how-to-install-the-dependencies-for-this-repo)
  - [How to follow style guidance](#how-to-follow-style-guidance)
  - [What is the philosophy around versioning the documentation?](#what-is-the-philosophy-around-versioning-the-document)
  - [How to explicitly identify support, stability, and dependency info](#how-to-explicitly-identify-support-stability-and-dependency-info)
  - [How to auto-format files](#how-to-auto-format-files)
  - [How to run the Assembly Workflow](#how-to-run-the-assembly-workflow)
  - [How to create a guide configuration](#how-to-create-a-guide-configuration)
  - [How to use DACX](#how-to-use-dacx)
- [Local development command reference](#local-development-command-reference)
  - [`yarn`](#yarn)
  - [`yarn build`](#yarn-build)
  - [`yarn start`](#yarn-start)
  - [`yarn assemble`](#yarn-assemble)
    - [`--cloud`](#--cloud)
    - [`--samples`](#--samples)
  - [`yarn format`](#yarn-format)
  - [`yarn snipsync`](#yarn-snipsync)
    - [`--clear`](#--clear)

## What is the goal of this README?

The goal of this README is to empower community contribution to the Temporal Platform information corpus, specifically docs.temporal.io. See [How to make changes to this repository](#how-to-make-changes-to-this-repository).

Maintainers and contributors to this project are expected to conduct themselves in a respectful way. See the [CNCF Community Code of Conduct](https://github.com/cncf/foundation/blob/master/code-of-conduct.md) as a reference.

This repository and its contents are open-source; individual and commercial use are permitted.

[MIT License](https://github.com/temporalio/documentation/blob/main/LICENSE.md)

### What is the “Temporal Platform information corpus”?

It is all of the Temporal Platform related information.
It includes the relevant information that might not be in this repository but can be found in other locations.

[Adding to this repository](#how-to-make-changes-to-this-repository) is only way to add to the information corpus.

We plan to offer more ways to add to the corpus.

Consider registering your already published information with Temporal IQ and reach out to us in Slack: https://temporalio.slack.com/archives/C05JRT1GKEE.

## What is in this repository?

This repository contains a large chunk of the Temporal Platform information corpus, divided into "source-of-truth" Markdown files and "generated" Markdown files, along with a changelog and an Assembly Workflow.
Each component is explained later in this README.

### `/docs-src` information nodes

This directory contains editable Markdown files, known as "information nodes," that are used to generate the guides seen in the `/docs` directory and on our website.
These nodes are registered to the Assembly Workflow with guide configurations. These configuration files can be found in `/assembly/guide-configs`.

### `/docs` generated files for Docusaurus

This directory contains the Markdown files that map directly to what you see in the documentation site. For example, `/docs/concepts/workflows` maps to [docs.temporal.io/workflows](http://docs.temporal.io/workflows).

However, most of these files are generated from `docs-src` information nodes based on the guide configurations in `assembly/guide-configs` .

Generated content has the following note below the metadata:

```html
<!-- THIS FILE IS GENERATED. DO NOT EDIT THIS FILE DIRECTLY -->
```

When suggesting changes to this repository, try not to highlight the errors in generated files.
Please make comments and suggestions in the appropriate source nodes.

### `/assembly` Assembly Workflow

The `/assembly` directory contains a Temporal Application written in JavaScript that uses Temporal's TypeScript SDK. This application acts as a build wrapper around the Docusaurus framework to assemble and generate the files in `/docs`.

Generated files are composed of many individual files, known as information nodes, contained in the `docs-src` directory.

Modularizing our content not only tidies up the repository but also provides a great example of Temporal at work. Our pre-build processes, a set of independent scripts, are invoked in a Temporal Workflow to generate and format what goes into Docusaurus.

Each JSON configuration file in [assembly/guide-configs](https://github.com/temporalio/documentation/blob/main/assembly/guide-configs) represents a user-facing narrative that pieces together concepts, how-to guides, and SDK-specific developer guides.

Run the Assembly Workflow with a local Cluster (such as [Temporal CLI](https://docs.temporal.io/kb/all-the-ways-to-run-a-cluster#temporal-cli)) or [Temporal Cloud](https://docs.temporal.io/cloud/).

See [How to run the Assembly Workflow](#how-to-run-the-assembly-workflow) for more information about this Workflow.

#### DACX information node generation tooling

Beyond creating generated files, the Assembly Workflow can generate information nodes for documentation code-sample repositories.
For more information, see [How to use DACX](#how-to-use-dacx).

### Snipsync code synchronization tooling

This repository is configured for [Snipsync](https://github.com/temporalio/snipsync), which checks in the snippets included throughout our documentation.
For more information, see [How to use Snipsync](#how-to-use-snipsync).

## How to get approval to create a pull request

We absolutely encourage contributions, but we need to know what you plan to change.

If you aren't part of the temporalio GitHub organization, [file a Github issue](https://github.com/temporalio/documentation/issues) to suggest a change.

If you are part of the temporalio organization, use Temporal’s internal tracking system to submit a work task to our team.

In both cases, you must receive approval from us before submitting a pull request. If you submit a pull request without proper approval, we will close it.

## How to fix a typo

**STOP! [Make sure you are eligible to create a pull request!](#how-to-get-approval-to-create-a-pull-request)**

**After receiving approval, follow these steps to make changes to this repository.**

For more information, refer to [How to make changes to this repository](#how-to-make-changes-to-this-repository).

```bash
git clone https://github.com/temporalio/documentation
cd documentation
yarn install
git checkout -b yourfix
```

Find the source node in the `docs-src` directory.

Make your changes in the *source files* named in the configuration file.
For instance, if you find a typo under "What is a Task?" (located in the Workers section of the Temporal docs website), open [docs/concepts/what-is-a-task.md](https://github.com/temporalio/documentation/blob/main/docs/concepts/what-is-a-task.md) and make the edit directly there.

Since the next set of steps requires a local Temporal Cluster, make sure that you have one running before you continue. We recommend executing the `temporal server start-dev` command to start a local cluster if you don't already have one running. If the cluster is not running, then the `./worker.js` command below will fail with a "Connection refused" error.

Open a new terminal. In the `assembly` directory, start the Worker.

```bash
cd assembly
yarn install
./worker.js
```

Open another terminal. Run the Workflow from the root of the repository.

```bash
yarn assemble
```

Format all files.

```bash
yarn format
```

Make sure you have a working build:

```bash
yarn build
```

Add your changes to the remote repository.

```bash
git add . && git commit -m "change deets"
git push origin yourfix
```

## How to make changes to this repository

**STOP! [Make sure you are eligible to create a pull request!](#how-to-get-approval-to-create-a-pull-request)**

**After receiving approval, follow these steps to make changes to this repository.**

If you want to fix a typo or something minor, check out the [How to fix a typo](#how-to-fix-a-typo) section of this README.

This section provides a higher-level view of the change proposal process, particularly for changes involving embedded code snippets or file generation.

1. Clone the documentation repository.
2. [Install the dependencies](#how-to-install-the-dependencies-for-this-repo).
3. Make changes to the information nodes in `docs-src`. Edit the guide configurations in `assemble/guide-configs` if files were added, deleted, or renamed.
   All changes must [follow our style guidance](#how-to-follow-style-guidance).
   1. See [How to construct a guide config](#how-to-create-a-new-guide-configuration).
   2. See [How to use DACX](#how-to-use-dacx).
   3. See [How to use Snipsync](#how-to-use-snipsync).
4. [Run the Assembly Workflow](#how-to-run-the-assembly-workflow).
5. [Format the files](#how-to-auto-format-files).
6. [Create a pull request](#how-to-create-a-pull-request).

### How to install the dependencies for this repo

**Before proceeding, make sure [Yarn](https://classic.yarnpkg.com/lang/en/docs/install/#mac-stable) and [Node.js](https://nodejs.org/en/download/) are installed. Make sure you install the latest version of Node.js (later than 18.0.0).**

On a Mac, use the command `brew install node@18`.

In the root directory of the repository, run `yarn` to install the packages needed to generate the `build` output. This includes the Docusaurus framework.

Change the directory to `assembly`. Run `yarn` to install the Assembly Workflow dependencies needed to generate the information nodes and their resulting guides.

### How to follow style guidance

The content included in the Temporal information corpus follows the [Google developer documentation style guide](https://developers.google.com/style), with deference to the [Microsoft Writing Style Guide](https://docs.microsoft.com/en-us/style-guide/welcome/) for issues left unanswered by Google.
In addition, we maintain a set of Temporal-specific style guidelines that override certain aspects of the Google and Microsoft guides.

We recommend that you use the Vale extension for your IDE. Vale allows you to configure rulesets in its own configuration file, making it useful for defining Temporal's style guidelines.

If you have Visual Studio Code, we recommend using the Vale VSCode extension.

For more information on general style rules, see the rulesets defined in the `vale/styles` configuration files.

#### Capitalization of core terms

Many of Temporal's core terms can be used in a generic way.

To differentiate one of Temporal's core terms from a generic instance of a term, always treat the Temporal term as a proper noun.

Do not capitalize generic versions of Temporal terms. Use generic versions sparingly to avoid confusion.

- Correct: "Next, register the Activity within the Workflow."

- Incorrect: "Next, register the activity within the workflow."

#### Abbreviation of "identifier"

Do not abbreviate the word "identifier" as "ID", "Id", or "id" unless it is part of a Temporal core term. For core terms, the correct abbreviation is "Id", such as in "Workflow Id" or "Activity Id".

- Correct: "You can provide an order identifier or customer identifier as a Workflow Id."

- Incorrect: "You can provide an order ID or customer id as a Workflow Id."

In code (and when quoting or referring to code in text), follow the conventions of each language.

#### En dashes in ranges

Using an en dash (`&ndash;` or the character `–`) for a range of numbers is acceptable.
However, we recommend using _from_, _to_, and _through_ instead of an en dash when possible.

Be consistent.
If you use an en dash in one range, use en dashes in all ranges.
Do not mix words and en dashes (or hyphens, for that matter).

- Correct: "5 to 10 GB"
- Correct: "5–10 GB"
- Correct: "5-10 GB"
- Incorrect: "from 5-10 GB"

#### Infinitive verb forms in headings

Use questions and infinitive verb forms for titles and headings. People tend to word their search queries with infinitive verb forms; aligning our titles with what's commonly searched improves SEO.

- Correct: "How to install Temporal"
- Incorrect: "Installing Temporal"

#### Infinitive verb forms in labels

Treat labels like headings or titles and use infinitive verb forms when possible.

- Correct: "Install Temporal"
- Incorrect: "Installing Temporal"

#### Sentence casing in headings

Use sentence casing for titles and headings.
Sentence casing means that only proper nouns and the first letter of the first word are capitalized.

- Correct: "How to get started with Temporal"
- Incorrect: "How To Get Started With Temporal"

### What is the philosophy around versioning the documentation?

The Temporal Platform includes many different components and core dependencies. Many components are independently versioned, meaning that they document their stability and support for their own dependencies. The [Temporal Go SDK reference](https://pkg.go.dev/go.temporal.io/sdk?tab=versions) provides a good example of document versioning.

The goal of this information set, in regards to versioning, is to remain “current”. That is, this information should serve the needs of the Platform’s user base as best it can based on what is/has recently happened across all the components of the Temporal Platform.

Whenever possible, we make explicit callouts to support, stability, and dependency information by using the `ssdi` metadata tag in `docs-src` nodes.

#### How to explicitly identify support, stability, and dependency info

Use the `ssdi` metadata in a `docs-src` node to explicitly call out support, stability, and dependency information.
Example:

```markdown
---
id: what-is-x
// ...
ssdi:
  - Introduced in [Temporal Server version x.xx.x](/path/to/release/notes/)
  - Available in [Temporal CLI version x.xx.x](/path/to/release/notes/)
  - Released in Temporal Cloud on <date>
---
```

### How to auto-format files

Run `yarn format`.

If you don't run this command locally before posting a pull request, a GitHub action runs the formatting.

### How to find broken links

[hyperlink](https://www.npmjs.com/package/hyperlink) is a command-line tool to find broken links.

In a terminal, run:

```bash
yarn check-links
```

This command will start the hyperlink checker.

### How to run the Assembly Workflow

Make sure you have a Namespace set up and ready to use.
We recommend using the [Temporal CLI development Server](https://docs.temporal.io/cli/#starting-the-temporal-server).

In a separate terminal, start the Temporal Server:

```bash
temporal server start-dev
```

In a separate terminal, run the Worker in the `/assembly` folder.

```bash
cd assembly
yarn install
./worker.js
```

In a third terminal, start the Workflow from the root of this repository.

```bash
yarn assemble
```

After the Workflow Execution is completed, run the auto-formatter.

```bash
yarn format
```

### How to create a guide configuration

Guide configurations are stored in `assembly/guide-configs`.

They are more or less organized to reflect the intended output in the `docs` directory; however, this arrangement is purely for file management purposes and does not affect where the guide output is written.

Guide configurations link independent source nodes together into a guide-style narrative.
Some of the guide-level metadata is supplied directly in the guide configuration, and the rest is generated from the source nodes. For example, the “keywords” and “tags” metadata are generated from the source nodes attached to the config.

Example configuration:

```json
{
  "file_name": "foundations.md", // name of the file that gets written to the docs directory
  "file_dir": "dev-guide/golang", // the directory that the file is written to
  "id": "foundations", // id of the document that is used by Docusaurus
  "slug": "/dev-guide/go/foundations", // slug of the document that is used by Docusaurus - OVERRIDES id + file_dir slug
  "title": "Go SDK developer's guide - Foundations", // title of the published document
  "sidebar_label": "Foundations", // sidebar label of the document
  "sidebar_position": 1, // position of the label in the sidebar
  "description": "The Foundations section of the Temporal Go SDK Developer's guide covers the minimum set of concepts and implementation details needed to build and run a Temporal Application in Go – that is, all the relevant steps to start a Workflow Execution that executes an Activity.", // Published description metadata
  "use_description": false, // uses the description as the first sentence in the generated doc
  "toc_max_heading_level": 4, // sets the maximum toc level for the generated doc
  "add_tabs_support": true, // adds mdx imports to generated doc to support tabs
  "sections": [
    // array of sections
    {
      "type": "p", // section type - p sections use the previous heading as an anchor
      "id": "go/foundations" // this is the directory + doc id within docs-src
    },
    {
      "type": "h2", // can go as deep as h4
      "id": "clusters/how-to-install-temporal-cli"
    },
    {
      "type": "h2",
      "id": "go/add-sdk"
    },
    {
      "type": "p",
      "id": "go/how-to-use-the-go-sdk"
    },
    {
      "type": "h3",
      "id": "go/api-reference-go"
    },
    {
      "type": "h3",
      "id": "go/code-samples"
    }
  ]
}
```

### How to use DACX

**What is DACX?**

DACX is short for “Docs as code.”
All the logic of the DACX tool is built into the Assembly Workflow.

**Why use DACX?**

DACX is similar to [Snipsync](https://github.com/temporalio/snipsync), except that DACX lets you write the documentation within the code.
Rather than curating the information and code within the documentation repository, you curate the information within the source code's repository.
The Assembly Workflow then generates Markdown guides from the narrative inlined with the code.
As a result, DACX provides rich content in multiple places.

To start using DACX, choose a source repo.

- Go → https://github.com/temporalio/documentation-samples-go
- Python → https://github.com/temporalio/documentation-samples-python
- Java → TODO
- TypeScript → TODO

You can also register a new source repo with the Assembly Workflow.

Add your repository in the `documentation_samples_repos` section of the [Assembly configuration](https://github.com/temporalio/documentation/blob/main/assembly/config.json) file.

Specify a branch name in the `ref` field if your changes are not merged into the main branch of the source repo.

Example:

```go
{
  "org": "your-org" // for us, this is temporalio
  "name": "your-repo" // name of the repository
  "ref" : "your-branch-name" // name of your branch; optional
  // if a branch is not specified, main is used
}
```

#### Identify DACX files

The Assembly Workflow identifies DACX files by the inclusion of `_dacx` in filenames.
In your source repository, add `_dacx` at the end of the file name, but before the file extension, to identify files using DACX.

For example, `my_main_workflow_file_dacx.go`.

This must be done for every file that contains DACX.

#### Write documentation using vanilla Markdown

In the source repository, write your documentation as Markdown in multiline comments.

The Assembly Workflow identifies all multiline comments as Markdown documentation.

Single-line comments are treated the same as the rest of the code.

Single-line commenting is still highly encouraged to make the code understandable.

For example:

```go
/*
This is treated as Markdown documentation.

Format this information as you would any other Markdown content.

Because Markdown maintains readability, anyone can read this, even when a [link](/somelink) is present.
*/

// MyCode is my code; this comment is treated the same as the code
func MyCode () error {
 // ...
}
```

#### Use `@dacx` comments to identify information nodes

In the source repository, add `@dacx` to a multiline comment at the bottom of the `_dacx` file.

The comment must be structured exactly like the following:

```go
/* @dacx
id: id-of-resulting-info-node
title: Title of the resulting info node
label: Info node label (often becomes the anchor if node is used as a header)
description: Longer description of the info node used in link page previews.
lines: line numbers
@dacx */
```

Example:

```go
/* @dacx
id: how-to-run-a-temporal-cloud-worker-in-go
title: How to run a Temporal Cloud Worker in Go
label: Run a Cloud Worker
description: Use a certificate key pair and your Temporal Cloud Namespace to connect to Temporal Cloud.
lines: 1-52, 64
@dacx */
```

#### Basic line-selection requirements

You must select both the opening multiline comment line and the closing multiline comment line when selecting multiline comments, either as a distinct selection or as part of a large group of Markdown and code.

For example:

```go
/*
This is treated as Markdown documentation.

Format this information as you would any other Markdown content.

Because Markdown maintains readability, anyone can read this, even when a [link](/somelink) is present.
*/

// MyCode is my code; this comment is treated the same as the code
func MyCode () error {
 // ...
}

/* @dacx
id: my-dacx-code-example
title: My DACX code example
label: DACX example
description: Always select the opening and closing multiline comment characters.
lines: 1-7
@dacx */
```

In the preceding example, we capture lines 1 through 7 to capture the whole multiline comment section.

We could also capture the full multiline comment as part of capturing a group of Markdown and code:

```go
/* @dacx
id: my-dacx-code-example
title: My DACX code example
label: DACX example
description: Always select the opening and closing multiline comment characters.
lines: 1-12
@dacx */
```

#### Use Assembly to generate the info nodes

In the documentation repository, run `yarn assemble --samples` to generate the information nodes into their respective language-specific directories.

For example, information nodes generated from `.go` files are generated into the `/docs-src/go` directory.

#### Add info nodes to an Assembly guide config

In the documentation repo, register the information node in a [guide configuration](https://github.com/temporalio/documentation/tree/main/assembly/guide-configs) file.

Arrange the information nodes to create a linear experience.

In many cases, information nodes are attached to developer guide configurations.

For example:

```json
{
  "file_name": "foundations.md",
  "id": "foundations",
  "file_dir": "application-development/golang",
  "title": "Go SDK developer's guide - Foundations",
  "sidebar_label": "Foundations", // This is the label of the guide in the sidebar
  "sidebar_position": 1, // This is the position within the sidebar relative to others at the same level
  "description": "The Foundations section of the Temporal Go SDK Developer's guide covers the minimum set of concepts and implementation details needed to build and run a Temporal Application in Go – that is, all the relevant steps to start a Workflow Execution that executes an Activity.",
  "use_description": false,
  "toc_max_heading_level": 4,
  "add_tabs_support": true,
  "sections": [
    {
      "type": "p", // use p for sections that you don't want a header for or want to string together.
      "id": "go/foundations"
    },
    {
      "type": "h2",
      "id": "go/add-sdk"
    },
    {
      "type": "h2", // this is the header level you want
      "id": "how-to-develop-a-workflow-in-go" // this is the id of the info node
    }
    // ...
  ]
}
```

#### Run Assembly one more time

In the documentation repo, run `yarn assemble` to generate the guides from the guide configurations.

Preview your work locally with the command `yarn start`.

### How to use Snipsync

Make sure your source code repository is registered in the `snipsync.config.yaml` file.

Place your [snipsync](https://github.com/temporalio/snipsync) snippet wrappers where you want the snippet to be within the information node.

```
<!--SNIPSTART typescript-hello-client -->
<!--SNIPEND-->

```

Run `yarn snipsync` to merge the snippet contents (in this case, from `[samples-typescript](https://github.com/temporalio/samples-typescript/blob/75bdcd613bd24f8f357cb96d1b83051353c5685a/hello-world/src/client.ts#L1)`) between the `SNIPSTART` and `SNIPEND` tags.

[Run the Assembly Workflow](https://www.notion.so/Documentation-repo-README-content-WIP-ddd0e586594246b3b9948919bbf7b12b?pvs=21).

Format the files with `yarn format`.

[Create a pull request](#how-to-create-a-pull-request).

### How to create a pull request

Commit your changes to a new branch.

If you are outside the Temporal organization, fork the repository and create pull requests from branches on your own fork.

See the README in GitHub's [first-contributions](https://github.com/firstcontributions/first-contributions) repo for guidance on forking and creating pull requests.

Vercel provides a deployment preview for each push to the pull request.

!https://github.com/temporalio/documentation/raw/main/static/img/readme/vercel-deploy-preview.png

If you don't belong to the Temporal organization, a member of the Temporal @temporalio/education team will need to approve your preview.

If you don't belong to the Temporal organization, you may need to sign the Contributor License Agreement yourself.

The @temporalio/education team tries to review GitHub issues and pull requests on a regular schedule.

When we merge your pull request, a new build automatically occurs and your changes publish to [https://docs.temporal.io](https://docs.temporal.io/).

#### Assembly Workflow merge conflicts

Run the Assembly Workflow to fix any merge conflict in ASSEMBLY_REPORT.md and any generated guides in the `/docs` directory.

Restart your Worker after merging `main` into your branch and before running Assembly again.

### How to preview the site locally

Run `yarn start`. This command starts a local development server and opens a browser window to [localhost:3000](http://localhost:3000/).

## Local development command reference

The following commands are available to aid in local development:

### `yarn`

This command ensures all the required dependencies are installed.

### `yarn build`

This command triggers a Docusaurus build and results in the browser-consumable JavaScript in the `/build` directory.
Note that the `/build` directory is ignored by Git.

### `yarn start`

This command spins up a local web server and serves the contents of the `/build` directory to [localhost:3000](http://localhost:3000/).

### `yarn assemble`

This command starts and runs the Assembly Workflow, provided you have a Temporal Cluster to connect to.

It accepts the following arguments:

#### `--cloud`

When provided, this flag causes the Temporal Client to connect to a Temporal Cloud Namespace. This flag requires that the following private information is set in `assembly/secure`:

- `docs-assembly.pem`: Paste and save the full CA certificate text.
- `docs-assembly.key`: Paste and save the full secret that was generated with the certificate.
- `cloud-connection.json`: Paste the following JSON, replacing the values with your own.

```
{
  "address": "your-cloud-address",
  "unique_id": "your-unique-custom-id",
  "namespace": "your-namespace"
}
```

#### `--samples`

When provided, this flag causes the Assembly Workflow to generate information nodes from the documentation samples repositories.

### `yarn format`

This command formats the documents per the `dprint.json` configuration.

### `yarn snipsync`

This command runs the Snipsync tool per the snipsync.config.yaml file.

### `--clear`

Run `yarn snipsync --clear` to remove the snippets.
