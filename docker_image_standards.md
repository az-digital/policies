Docker Image Standards for AZ Digital
=====================================

## Git Controls Image Definitions

The Dockerfiles and any other files that define images used in AZ Digital
projects must be under version control in a Git repository, just like any other
code that a project uses, and subject to the same code review process as
everything else: any changes must start with a pull request, be approved by at
least two independent reviewers, and only then merged with an active working
branch in the Git repository. No revised Docker images should appear in the
corresponding Docker image repository in advance of the Git-versioned source
file revisions.

## Types of Docker Images

There are four general types of Docker image for use in AZ Digital projects,
primarily for use as steps in CI operations, or for local development.
Additional types are possible, for example for containers that can function as
general-purpose command-line tools to run specific software, but these are
less relevant to most projects. 

### 1. Project-specific

Some images are very closely linked to a specific project, and not much use
anywhere else.

#### 1.1. Dockerfile location

The Dockerfiles that define project-specific images should be part of the
project's own Git repository.

#### 1.2. Image naming

Project-specific Docker images should have names derived from the project's
own name.

#### 1.3. Image tagging

Image tags should follow the same conventions as the project's own Git tag and
branch names.

### 2. Standard environment

Some images package a particular set of tools, creating standardized build or
test environments.

#### 2.1. Dockerfile location

The Dockerfiles and anything else that defines the images should be in a
freestanding Git repository.

#### 2.2. Image naming

Image names, and the names of the parent Git repository, should start with the
`az` or `arizona` prefix, and be in some way descriptive of the set of tools
provided.

#### 2.3. Image tagging

Image tags should follow a semver pattern, with three sets of digits `3.1.4`
and no leading `v` character.

### 3. Base image

Some images provide are not intended to be used directly, but to provide a
base image which other Docker images can use as a starting point for their
own definitions.

#### 3.1. Dockerfile location

The Dockerfiles and anything else that defines the images should be in a
freestanding Git repository, unless the base image will be very tightly linked
to a set of related images, and unused for anything else.

#### 3.2. Image naming

Image names, and the names of the parent Git repository, should start with the
`az` or `arizona` prefix, be in some way descriptive of the set of tools
provided, and end with the suffix `base`.

#### 3.3. Image tagging

Image tags should follow a semver pattern, with three sets of digits `3.1.4`
and no leading `v` character.

### 4. Kitchen sink image

For the early stages of development in some projects it might be helpful to
have images that pre-package a wide range of different tools, more like
what one would expect from a development VM than a Docker container; use
in the later stages of development and production should be discouraged.

#### 4.1. Dockerfile location

The Dockerfiles and anything else that defines the images should be in a
freestanding Git repository.

#### 4.2. Image naming

Image names, and the names of the parent Git repository, should start with the
`az` or `arizona` prefix, and include the name or version codeword of the
system that forms the ultimate base of the image (such as `buster` for
Debian 10).

#### 4.3. Image tagging

Image tags should follow a semver pattern, with three sets of digits `3.1.4`
and no leading `v` character.

## Automated Builds

Wherever possible, building a new tagged Docker image and making it available
from a Docker repository should be an automated process started by pushing a
new tag to a Git repository.

## Public Repositories

Generally all Docker image repositories should be public rather than private,
but protected from unauthorized changes (such as substituting an image that
had the same name and tag as an existing one, but was modified to include a
Bitcoin miner), and not including any sensitive information such as passwords
or private keys.

## `latest` Considered Harmful

All images should have an explicit tag. Trying to use an image without a tag
should result in an error, not a default image (or equivalently the one tagged
`latest`). This is both to enforce more deterministic behavior on whatever is
using the images, and to avoid the extra setup that might be required for
automated build systems to update a `latest` tag to always be an alias for the
image with the highest-numbered tag.
