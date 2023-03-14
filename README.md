# Hello World JavaScript Actions

This project is to help me learn about building GitHub Actions.

For the source of this information see the [Creating a JavaScript action](https://docs.github.com/en/actions/creating-actions/creating-a-javascript-action) documentation.

## How to Build

The source information talked about checking in the node_modules. We ran into an interesting issue with this. The npm commands were run in a [dev container](https://code.visualstudio.com/docs/devcontainers/containers). The `npm install @actions/core` created a 'uuid' file in the 'node_modules/.bin' folder. This file is a symbolic link to another file in the node_modules folder. If we try to do a `git add .` on our windows machine it fails to add this file with a fatal error. On windows the 'uuid' file is a zero byte file.

If we run the `npm install @actions/core` command on the windows machine it creates different files in the 'node_modules/.bin' folder. We question if these checked in node_module files installed from a windows machine would run successfully on a Linux runner. 

As instructed we compiled the code into a dist folder using `ncc build index.js --license licenses.txt`. Contrarily to the instructions, we did not include the node_modules folder in our checkin.

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