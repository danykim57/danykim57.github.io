---
title:  "Kubernetes: Abstract"
header:
  teaser: "https://farm5.staticflickr.com/4076/4940499208_b79b77fb0a_z.jpg"
categories:
  - BackEnd
tags:
  - BackEnd
---

Monolithic vs Microservice

When we first learn to program, we just learn how to print "Hello World".

In most languages, it is just a single line to print simple "Hello World" on the console.

However, when the program contains more than a hundred lines, I feel obligated to use function and class for chopping the program into digestible size for my brain. It is easier to understand any code with functions and classes rather than with a single function. Programmers spend more than 85% of their time debugging. We all have experience searching for a misplaced comma in the middle of lines of codes. To reduce bugs and improve productivity, we use IDE or other tools that help prevent human error.

In the case of building applications, monolithic architecture style means you just put every chunk of codes without much separation. Due to the size of code, it is more prone to mistakenly make bugs and it is excruciating to fix the bugs. Therefore, using the microservice architecture style is more resistant to errors and bugs. Microservice consists of small apps(components). Even when one app is down, the event only affects other apps' directly related features. Unlike microservice, a monolithic system will face a system failure. Microservice sounds whole and all good, but it has similar problems to concurrency. Handling a microservice system is a tough job to go on. Let's think of Metcalfe's Law. Metcalfe's law is that network's value is proportional to the number of nodes. This law is not only about that the value goes up but also that system's complexity increases. This is a reason why company runners love microservice, but software engineers feel agonized with it.


{% include figure image_path="/assets/images/MonoMicro.jpg" alt="MonoMicro" caption="Monolithic vs. Microservice" %}

Some would say to me, "Hey, you just said we need to use microservice for reducing bugs and errors. If the complexity goes up, it will be harder to solve the problems on the system." Microservice system is harder to administer out of configurations. Some people already solved the problem.

Kubernetes is a container orchestration system for automation of deployment, scheduling, failure handling, and monitoring. Kubernetes(K8s) is strongly supported by the Google community for over 15 years.

Kubernetes manages containers which is a set-up environment for applications. It doesn't matter how different hardware, OS, libraries, and computer languages each application use, containers will fully support required environments for apps.

One would say "Hey, we already have VM(virtual machine) to run a required environment. why would we need to use K8s?" Yes, it is possible to run apps with VMs, but VM costs some computer resources like CPU usage and memory. K8s is capable of doing the same job at a lower cost with Linux container technology. Linux container technology makes a process run like inside the host's OS. Therefore, it costs fewer overheads. Also, Linux container technology still provides isolation for each container.

What is Docker and why does Kubernetes come along with it?

{% include figure image_path="/assets/images/docker-vm-container.webp" alt="docker" caption="docker" %}

Docker is a container platform. Kubernetes is a container orchestration system. Kubernetes only supported Docker, when they first came out. There are other open-source container platforms such as rkt(rock-it). Kubernetes is not stuck in Docker.

Kubernetes use Master-Workers architecture. Master node handles the Kubernetes control plane to manage the whole system. Worker nodes run applications.

This post is an abstract of Kubernetes. I wouldn't go further than here, but I hope this was helpful to understand what is Kubernetes.





References:

Kubernetes in Action by Marco Luksa

[alex_barashkov: microservices vs monolith architecture](https://dev.to/alex_barashkov/microservices-vs-monolith-architecture-4l1m)

[ZDnet: What is docker and why is it so darn popular](https://www.zdnet.com/article/what-is-docker-and-why-is-it-so-darn-popular/)

  

