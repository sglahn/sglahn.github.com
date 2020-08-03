---
layout: post
title: "Released gradle-dockerfile-plugin version 0.3"
date: 2017-04-18 21:00:26 +0200
category: [Docker, Java]
featured-img: gradledocker
---
I have just released version 0.3 of my <a href="https://github.com/sglahn/gradle-dockerfile-plugin">gradle-dockerfile-plugin</a>. This is a Gradle plugin to build and push Docker images using an external Dockerfile. This means there is no need of inline configuration for Docker in the Gradle build file. The plugin is available through <a href="https://search.maven.org/#search%7Cgav%7C1%7Cg%3A%22org.sglahn%22%20AND%20a%3A%22gradle-dockerfile-plugin%22">Maven Central</a> and the <a href="https://plugins.gradle.org/plugin/org.sglahn.gradle-dockerfile-plugin">Gradle Plugin Portal</a>.

### Installation

To use the plugin add a build script dependency to your Gradle build file:
{% highlight groovy %}
buildscript {
    repositories { mavenCentral() }
    dependencies { classpath('org.sglahn:gradle-dockerfile-plugin:0.3') }
}
apply plugin: 'dockerfile'
{% endhighlight %}

or via the new plugin mechanism introduced in Gradle 2.1:
{% highlight groovy %}
plugins {
  id "org.sglahn.gradle-dockerfile-plugin" version "0.3"
}
{% endhighlight %}

The plugin will add the following tasks to your project:
{% highlight bash %}
$ ./gradlew tasks
Docker tasks
------------
dockerBuild - Builds a new image with a Dockerfile.
dockerPush - Pushes a docker image to a repository.
{% endhighlight %}

### The dockerBuild task

The dockerBuild task will build a new Docker image. The default settings are:
- dockerfile: ${projectDir}/Dockerfile.
- imageName: project.name
- tags: project.version and latest.
For more information see Configuration section.

### The dockerPush task

The dockerPush task will push the Docker image to a Docker repository. If authentication is required use <a href="https://docs.docker.com/engine/reference/commandline/login/">docker login</a> to add the credential to your $HOME/.docker/config.json file. <a href="https://hub.docker.com/r/sglahn/gradle-dockerfile-plugin-example-project/">This</a> is how it looks like when the example project pushed to DockerHub.

### Configuration

The following configuration can be added to your Gradle build file:

{% highlight groovy %}
docker {
    // Image version. Optional, default = project.version
    imageVersion = "version"
    // Image name. Optional, default = project.name
    imageName = "name"
    // Docker repository. Optional, default == no repository
    dockerRepository = 'sglahn'
    // Path to the Dockerfile to use. Optional, 
    // default = ${projectDir}/Dockerfile
    dockerFile = 'src/main/docker/Dockerfile'
    // Add a list of tags for an image. Optional, default = $applicationVersion
    tags = [version, 'latest', 'Hello']
    // Set metadata for an image. Optional, default = no label applied
    labels = ['branch=master', 'mylabel=test']
    // name and value of a buildarg. Optional, default = no build arguments
    buildArgs = ['http_proxy="http://some.proxy.url"']
    // Always remove intermediate containers, even after unsuccessful builds. 
    // Optional, default = false
    removeIntermediateContainers = true
    // Isolation specifies the type of isolation technology used by containers. 
    // Optional, default = default
    isolation = 'default'
    // Do not use cache when building the image. Optional, default = false
    noCache = true
    // Always attempt to pull a newer version of the image. 
    // Optional, default false
    pull = true
    // Suppress the build output and print image ID on success. 
    // Optional, default = true
    quiet = false
}
{% endhighlight %}

<span itemprop="author" itemscope itemtype="https://schema.org/Person">
    <meta itemprop="name" content="Sebastian Glahn" />
    <meta itemprop="url" content="http://blog.tldnr.org" />
    <meta itemprop="sameAs" content="https://twitter.com/sglahn" />
    <meta itemprop="sameAs" content="https://github.com/sglahn" />
    <meta itemprop="sameAs" content="https://www.thingiverse.com/sglahn" />
    <meta itemprop="sameAs" content="https://stackoverflow.com/users/1453205/sglahn" />
</span>
