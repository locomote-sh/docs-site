---
title: Content Builds
layout: documentation
---
# Content builds

_Locomote_ provides a build system with a comprehensive set of builds tools which can be used to generate publishable content from source files and data. The build tools perform a lot of the functions you’d expect - such as generating HTML from markdown - but also automate a lot of the tasks that need to be done when preparing pages to be published as PWAs.

_Locomote_ build tools can also be used with standard, third-party tools and website builders such as _WebPack_ or _Jekyll_.

# Build workflows

The build system is a workflow system which uses the different branches of a content repository to store and represent state. A workflow is modelled as a sequence of one or more workflow steps. Each workflow step is modelled as one or more build actions applied to a source branch of the content repository, and writing output to a target branch:

![Workflow step]({{ site.baseurl }}/content-management/workflow-step.svg)

The workflow step is triggered whenever content is pushed to its source branch.

Source and target branches are normally in the same content repository, but workflows can also target other accessible repositories (i.e. repos in the same _Locomote_ account). Workflow steps can be chained, with updates to the target branch triggering follow on workflows which have the target branch defined as their source branch.

The default _Locomote_ build workflow is composed of a single workflow step which uses the _master_ branch as its source branch and the _public_ branch as its target branch, and which applies the the standard _Locomote_ build operations to the source.

# Server side builds

A server side build is triggered whenever updates are pushed from a cloned repository to the server, with the standard build workflow being applied to the received updates.

Server side builds can only use the standard _Locomote_ build operations; if third-party build tools are needed then builds must be performed locally (see following section).

# Local builds

Local builds can be performed by executing the build actions locally on the developer’s machine and then afterwards publishing the build result by pushing the target branch to the server. This can be useful when testing and debugging a site, and is necessary if third-party build tools are being used. Even when using external build tools, builds should be configured and coordinated through the _Locomote_ build workflow in order to ensure that workflow state is properly recorded.

## Configuring workflow

Custom build workflows can be defined using the `“workflow”` section of the repository manifest file (i.e. `locomote.json`). This section of the manifest contains a list of the names of the workflow source branches in the repository, mapped to the build actions that should be invoked when the source branch is updated, and the name of the target branch where build results should be written to.

The following JSON snippet shows a simple sample build workflow which is triggered by updates to the master branch and sends build output to the public branch:

```json
    {
        “workflow”: {
            “master”: {
                “action”: “build”,
                “target”: “public”
            }
        }
    }
```

A build action can be any command available on the system path.

## Running workflow

The `locomote-build` command is used to locally run build workflows.

Install the build command using _npm_:

```
npm install -g @locomote.sh/build
```

To use the build command, run it from within a cloned copy of the source branch of your content repository:

```
locomote-build workflow
```

The command will execute the build workflow step appropriate for the source branch. In doing this, it will clone a copy of the target branch to a temporary location, write the build result to it, and automatically commit the changes and push them back to the server.

# Website builds
The default build tools include a static website builder called `locomote-heckle`. _Heckle_ takes a set of markdown files as input and combines them with theme and layout files to generate a set of HTML pages.

_Heckle_ is compatible with the popular static site builder _Jekyll_. It uses the same template language (Liquid) and the same configuration, layout and include file formats. The main difference is that _Heckle_ uses _node.js_ modules for custom tags, filters and other extensions; _Jekyll_ plugins (being _Ruby_ based) won’t work with _Heckle_.

# Webpack builds

_Webpack_ builds can be incorporated into a build workflow, but have to be executed locally so that all of the build dependencies are available.

# Development server
The _Locomote_ build tools include a development HTTP server which is useful for testing and debug during the development phase. The development server is a _node.js_ process which is run locally and which serves content from a build target branch. The development server will detect changes in the source branch and automatically execute builds.
