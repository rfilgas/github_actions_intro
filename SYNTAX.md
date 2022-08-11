# Notes
## Files and structure:

* Yaml comes in two files extensions: .yml and .yaml
* Formatting consists of indentation, colons and dashes.
* a workflow name is not required, but can be added. Github will make a default name if not using the path to the workflow file and the name of the file itself.


```yaml
name: Hello World
on: push
jobs:
	say-hello :
	runs-on: ubuntu-latest
	steps :
	- name : Checkout
		uses : actions/checkout@v2.0.0
	- run: echo Hello, World!
```

* The name of each job essentially defines an object.
* Each name can have a runs-on.
* A uses statement defines a workflow

## Events

There are several types of repository events, a few include:

* push
* pull-request
* release
* merge

## Webhooks

Github webhooks are events associated with the repo, but may not have a direct relation to a code change.

* Branches being created or deleted
* Issues being opened or resolved
* When mebers join or leave a project

## Workflow Rules:

* **Workflows** can be run on a schedule using a similar to cron format.
* The **jobs** keyword defines a block that lists the jobs for the workflow.
* Each workflow must have at least one **job**. This is the hello-world above.
* **Jobs** must start with a letter or underscore and must be alpha numeric.
* The **runs** on attribute describes the OS for the virtual environment. This a runner.
* Github allows for self-hosted runners.
* The **steps** attribute lists specific actions and commands.
* The **uses** attribute uses an action - a bundle of code/docker image. They live in the same directory as a workflow or a container registry.
* The **run** attribute executes a command or series of commands that can be used in a shell environemnt.
* Actions within the same repo can be used with relative paths: -uses: "./.github/my_action"
* To call an action from a different repository the UID, repo, and reference is needed. -uses: "username/repo_name@branch_name_or_SHA"

## Conditional Examples:

```
# Trigger on push
on:
	push:
		branches:
			- develop
			- master
---------------------------------
# Trigger on Pull
on:
	pull_request:
		branches:
			- master
---------------------------------
# Trigger on both conditionals.
on:
	push:
		branches:
			- develop
			- master

	pull_request:
		branches:
			-master
```

Other conditionals include branches, tags, and special charctars using regex:
* branches-ignore
* tags tags-ignore
* 'feature/new-feature'
* 'release/*' 'm?ster'

## Limitations
* Only 20 workflows can be running at the same time.
* concurrent jobs are limited to 20 concurrent jobs for free plans which goes up by tier.
* obs are limited to 6 hours max
* Actions can't trigger other workflows - this would allow infinite loops.
* Action logs are limited to a max of 64 kilobytes
* Jobs will queue or fail if limits are exceeded.

## Deployable prewritten actions:
https://github.com/marketplace?type=actions

```yaml
# code from tutorial
    steps:
    - name: Check out the code
      uses: actions/checkout@v2
    - name: Python Syntax Checker
      uses: cclauss/Find-Python-syntax-errors-action@v0.2.
```
## Passing arguments to an action:
```yaml
steps:
	name: code_checkout
	uses: actions/checkout@v2
	with:
		repository: apache/tomcat  # checkout all code from apache tomcat repo
		ref: master # branch
		path: ./tomcat # relative path from the branch

```