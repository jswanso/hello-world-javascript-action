# Hello World JavaScript Actions

This project is to help me learn about building GitHub Actions.

The instructions I used for created this are from GitHub Action's [Creating a JavaScript action](https://docs.github.com/en/actions/creating-actions/creating-a-javascript-action) documentation.

## Lessons Learned and Modifications

I wrote a quick summary of the lessons I learned from this exercise and some of the modifications I did.

### Node Modules Checkin Issue with Dev Containers

The source information talked about checking in the node_modules to GitHub. I ran into an interesting issue with this because I decided to add a [dev container](https://code.visualstudio.com/docs/devcontainers/containers) to this project so I could run the commands in a docker container instead of on my local machine. The npm commands to install the two required packages and later the command to globally install the [vercel/ncc](https://www.npmjs.com/package/@vercel/ncc) compiler were all run in my locally running dev container.

The dev container runs a Linux container and when running `npm install @actions/core` command in the container it created a 'uuid' file in the 'node_modules/.bin' folder. This file is a symbolic link to another file in the node_modules folder. If I then try to do a `git add .` on my windows machine it fails to add this file with a fatal git error. On windows the 'uuid' file is a zero byte file and seems to be the reason `git add` fails to add this.

If I run the `npm install @actions/core` command on the windows machine it creates a different set of files in the 'node_modules/.bin' folder. I question if these checked in node_module files installed from a windows machine would run successfully on a Linux runner. Maybe this is one of the scenarios that the instructions are referencing when they say 'checking in your node_modules directory can cause problems'.

As instructed I compiled the code into a dist folder using command `ncc build index.js --license licenses.txt`. Contrarily to the instructions, I did not include the node_modules folder in my GitHub check-in. Looking at other market place actions such as [upload artifact](https://github.com/actions/upload-artifact), it doesn't look like they have node_modules checked in either.

### Node Version 18

I tried to use node18 in the [action.yml](action.yml) file, but at the time of testing it was not supported yet when running the action. Below is the error message I received.

```text
System.ArgumentOutOfRangeException: Specified argument was out of the range of valid values. (Parameter ''using: node18' is not supported, use 'docker', 'node12' or 'node16' instead.')
   at GitHub.Runner.Worker.ActionManifestManager.ConvertRuns(IExecutionContext executionContext, TemplateContext templateContext, TemplateToken inputsToken, String fileRelativePath, MappingToken outputs)
   at GitHub.Runner.Worker.ActionManifestManager.Load(IExecutionContext executionContext, String manifestFile)
```

The usage of this action can be seen in my [Hello World Action](https://github.com/jswanso/GitHubActionsTest/blob/main/.github/workflows/hello-world-action.yml) workflow.

## Description

This action prints "Hello World" or "Hello" + the name of a person to greet to the log.

## Inputs

### `who-to-greet`

**Required** The name of the person to greet. Default `"World"`.

## Outputs

### `time`

The time we greeted you.

## Example usage

```yaml
uses: actions/hello-world-javascript-action@v1.1
with:
  who-to-greet: 'Mona the Octocat'
```
