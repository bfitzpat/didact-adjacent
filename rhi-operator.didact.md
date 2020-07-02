# How to install RHI Operator

The RHI Operator is a Kubernetes Operator based on the Operator SDK for installing and reconciling RHI products. It currently supports:

* Red Hat Integration - AMQ Streams
* Red Hat Integration - API Designer (Apicurio Operator)
* Red Hat Integration - Camel K (coming soon!)
* Red Hat Solution Explorer

[Find more about the operator at the GitHub project.](https://github.com/redhat-integration/rhi-operator)

## Prerequisites

* You must have an empty OpenShift cluster available with a good deal of resources available. One thing we noticed is that if any version of the Solution Explorer operator exists on the cluster, the RHI operator will not move past that step. 
* You must have the Go language installed ahead of time on your system. [See the Golang downloads page for details.](https://golang.org/dl/)

## Install steps

1. Open a new Terminal to execute a series of commands ([^Execute this](didact://?commandId=vscode.didact.startTerminalWithName&text=RHIOperatorTerminal "Create a new terminal window called 'RHIOperatorTerminal'"){.didact})
2. Change to the directory in which you want to clone the project.
3. To clone the RHI Operator, run `git clone https://github.com/redhat-integration/rhi-operator` ([^Execute this](didact://?commandId=vscode.didact.sendNamedTerminalAString&text=RHIOperatorTerminal$$git%20clone%20https://github.com/redhat-integration/rhi-operator "Call `git clone https://github.com/redhat-integration/rhi-operator` in the terminal window called 'RHIOperatorTerminal'"){.didact})
4. To move into the new `rhi-operator` directory, run `cd rhi-operator` ([^Execute this](didact://?commandId=vscode.didact.sendNamedTerminalAString&text=RHIOperatorTerminal$$cd%20rhi-operator "Call `cd rhi-operator` in the terminal window called 'RHIOperatorTerminal' to move into the operator project"){.didact})

Next, the following commands must be run to avoid a [known issue](https://github.com/matryer/moq/issues/98) related to the Moq package:

5. Compile the project with `make code/compile` ([^Execute this](didact://?commandId=vscode.didact.sendNamedTerminalAString&text=RHIOperatorTerminal$$make%20code/compile "Call `make code/compile` in the terminal window called 'RHIOperatorTerminal' to compile the project"){.didact})
6. Install the `Moq` command into Go by running `go install github.com/matryer/moq` ([^Execute this](didact://?commandId=vscode.didact.sendNamedTerminalAString&text=RHIOperatorTerminal$$go%20install%20github.com/matryer/moq "Call `go install github.com/matryer/moq` in the terminal window called 'RHIOperatorTerminal' to install `moq` in Go"){.didact})
7. And then you must add the Go/bin folder (`$HOME/go/bin`) to the system path (Run: `export PATH=$PATH:$HOME/go/bin` on linux) ([^Execute this](didact://?commandId=vscode.didact.sendNamedTerminalAString&text=RHIOperatorTerminal$$export%20PATH=$PATH:$HOME/go/bin "Call `export PATH=$PATH:$HOME/go/bin` in the terminal window called 'RHIOperatorTerminal' to add the Go/bin folder to the system path"){.didact}))

And once Moq is in place, you can use Go scripts to install the operator:

8. Run `make cluster/prepare/local` to prepare the cluster for the installation ([^Execute this](didact://?commandId=vscode.didact.sendNamedTerminalAString&text=RHIOperatorTerminal$$make%20cluster/prepare/local "Call `make cluster/prepare/local` in the terminal window called 'RHIOperatorTerminal' to prepare the cluster for the rhi operator"){.didact})
9. Run `oc create -f deploy/crds/examples/rhmi.cr.yaml` to create a custom resource to kick off installation of RHI products once the operator has started ([^Execute this](didact://?commandId=vscode.didact.sendNamedTerminalAString&text=RHIOperatorTerminal$oc%20create%20-f%20deploy/crds/examples/rhmi.cr.yaml "Call `oc create -f deploy/crds/examples/rhmi.cr.yaml` in the terminal window called 'RHIOperatorTerminal' to create a RHI custom resource"){.didact})
10. Run `make code/run` to kick off the installation ([^Execute this](didact://?commandId=vscode.didact.sendNamedTerminalAString&text=RHIOperatorTerminal$$make%20code/run "Call `make code/run` in the terminal window called 'RHIOperatorTerminal' to start the install process"){.didact})

Have a nice meal. It's going to take a while.
