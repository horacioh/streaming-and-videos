# Automation tips from John Lindquist

## create a template repo from codesandbox

- create a sandbox (whatever template)
- create a repo from the sandbox
- go to github and copy the link of the repo origin
- in the command line: `npx degit <GIT_REPO>`
- clone the project without any git info (you can then `git init`!

## Use `hub` to create repos on github directly from the command line

- add a command line alias `alias git='hub'`

## create ZSH functions

```zsh

+vanilla() {
    if [ "$1" = "" ]
    then
        echo Please pass a name for the project
        return
    fi
    cd ~/projects
    npx degit <USERNAME>/<REPO_NAME> "$1"
    cd "$1"
    git init
    git add .
    git commit -m "first commit"
    git create
    git push -u origin master
    pnpm install
    teach .
    yarn start
}
```

- you can add an "if" statement to check for the required name

## Create an Alfred workflow that runs that command

https://egghead.io/lessons/egghead-launch-a-new-lesson-from-a-keyboard-shortcut?pl=john-lindquist-s-ultra-advanced-yak-shaved-lesson-creation-process-dd3b

## install a local package globally for simple use
