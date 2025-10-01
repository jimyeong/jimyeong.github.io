---
layout: single
title: "Docker chapter 2, Working with image"
date: 2025-09-30 00:00:00 +0000
categories: [Tech]

tags:
  - Docker
  - Infrastructure
---

## Working with Image

#### 1. Docker Image - The TLDR
    * Docker create images by sucking independent layers and representing them as single unified object  

    * One layer might have the OS components another one might have application dependencies

#### 2. Intro to Images
    * Images are build-time constructors, whereas containers are run-time constructs  

    * You can delete an image when any containers from this image are left running. You should stop and delete containers first to remove an image  

    * Containers are designed to run a single application or Microservices, so you shouldn’t include non essentials such as build tools or trouble shooting tools, (ex Alpine linux, 3MB,) it’s common for images not to carry shells or package manager.
#### 3. Pulling Images
    * Process of getting images is called pulling. (ex) $docker pull Redis  

    * There are two reasons to pull 
        1. Pulling image tagged as latest
        2. Pulling image from docker Hub  
        
    * Docker is clever enough only to pull the layers it doesn’t already have
#### 4. Image registries
    * You can use Docker CLI & API tools thanks to OCI registries( ie. Docker Hub , 3rd parties
#### 5. Official repositories
    - to be continued 
