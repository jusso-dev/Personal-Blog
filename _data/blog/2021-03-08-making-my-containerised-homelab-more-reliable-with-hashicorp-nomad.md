---
template: BlogPost
path: /supercharging-docker-with-hashicorp-nomad
date: 2021-03-08T10:23:55.792Z
title: 'Making my containerised Homelab more reliable with Hashicorp Nomad '
metaDescription: 'containers, docker, Hashicorp, Nomad, homelab'
thumbnail: /assets/featured.jpg
---
# Portainer VS Hashicorp Nomad, how do they stack up?

Ok to start with, they are not the same product offering and both perform very different functions, well some the same but are the same but we will get into that..

## What is it?

From the Hashicorp Nomad site:

> A simple and flexible workload orchestrator to deploy and manage containers and non-containerized applications across on-prem and clouds at scale.

So what does this mean in practice? well it means you can use Nomad as a service orchestrator in front of common services like Kubernetes or Docker as well as for standard executable binaries.

## How I replaced my Docker container workloads

To start with Hashicorp and Docker I feel personally is the easiest way to get up and running, especially if you are already familiar with Docker. The first step is to translate your Docker workload files, whether that be Docker files or docker-compose files to [Hashicorp Language Markup ](https://www.nomadproject.io/docs/job-specification)syntax or .nomad/.hcl file syntax.

An example of a Nomad jobspec file could be as simple as below, which translates the basic requirements for the oznu/homebridge Docker image for homebridge:

```
job "homebridge" {
  datacenters = ["dc1"]

  group "homebridge" {
    network {
      port "http" {}
    }  
    task "server" {
      driver = "docker"

      config {
        network_mode = "host"
        image = "oznu/homebridge:latest"
        ports = ["http"]
        volumes = [ "$(pwd)/homebridge:/homebridge" ]
      }
      env {
          PGID = "1000"
          PUID = "1000"
          HOMEBRIDGE_CONFIG_UI = "1"
          HOMEBRIDGE_CONFIG_UI_PORT = "8581"
          TZ = "Canberra/Australia"
      }
    }
  }
}
```

As you can see above, it's fairly intuitive (at least I think so at least) but let's break down the file above:

The section titled "Task" is the meat of the config, there are plenty of other write ups online about Nomad so I won't deep dive here, but essentially the Task section will define how our Docker file should work behind the Nomad service orchestrator.

First, we define what type of "Task" we want Nomad to stand up for us, in this case, we define the driver to be "docker" so it uses, you guessed it, docker under the covers to spin up our job.

Then, we set a few docker housekeeping items, some relevant to homebridge but nonetheless important for the image to work:

"network_mode" basically tells Nomad, to tell Docker under the covers to let the image use the host networking mode, this is important for homebridge and many other Docker based workloads.

"image" is the Docker image we want to pull and use.

"ports" tell Nomad to expose http based ports.

"volumes" sets the Docker volumes are container image requires.

Finally, we define a "env" config section to pass relevant environment variables to our workload.

## What have I learned from switch to Nomad?

Well, first of all I'll be honest, when I first saw the tool I struggled to understand how I would benefit from using it over just vanilla Docker or Kubernetes, but then it clicked, that Nomad just works. The tool has a little steep learning curve initially and initial deployment could have been smoother but overall once you get in the swing of using it you'll understand how powerful, stable and reliable it can be. Since I have switched, I have not had to restart it once or really even touch it. It just works. 

## Next steps

Nomad is great for use with Docker and Kubernetes but it can also be used to add a layer of service orchestration to legacy workloads. For example, let's say your company has a legacy ASPX WebForms website or legacy Java Springboot site that exposes WSDL SOAP services, Nomad can be used to modernise the deployment of said applications, without rewriting the application. How you ask? well it's simple, remember above when I said the Task definition for a Nomad jobspec allows you to define a driver, and we set ours to "docker"? well if you switch this object to "exec" Nomad will run a executable for you, which could be a legacy web/business application, without touching a single line of code.

Here's an example jobspec file for a raw binary:

`task "example" {   driver = "exec"`

`  config {     command = "name-of-my-binary"
  }`

`  artifact {     source = "https://internal.file.server/name-of-my-binary"
    options {
      checksum = "sha256:abd123445ds4555555555"
    }
  }
}`

So you can see from above how easy it could be to replace that jobspec that fits your needs and adds robust, powerful service orchestration to a legacy business application. If your legacy business application can be run as a single binary, there's a good change that Nomad can add a layer of service orchestration to it.

Until next time, stay informed and stay safe out there!
