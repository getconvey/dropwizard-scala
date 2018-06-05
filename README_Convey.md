Building and deploying to S3
============================

There are SBT plugins to deploy to S3, but unfortunately, the ones
I tried do not generate and update `maven-metadata.xml` files, which
results in errors when Maven attempts to resolve the artifacts.  Because
of that, we use SBT to build and publish the artifacts (including the 
pom files) and Maven to do a second-pass deploy that generates the
`maven-metadata.xml` files.

Updating the version
--------------------

Set the version in `project/Project.scala`.  If you are deploying a 
release, then after you merge, tag the commit.  Be sure to push the tag; 
deployment will work even if you forget to push!  If you are releasing
a snapshot, you don't need to tag the commit.

First pass with SBT
-------------------

```$bash
$ sbt clean publish
[info] Loading project definition from /Users/david/dev/dropwizard-scala/project
(etc.)
[success] Total time: 35 s, completed Jun 5, 2018 6:16:17 PM
```

This builds and publishes the artifacts, but without the `maven-metadata.xml` 
files.

Second pass with Maven
----------------------

Note that the version used in this step must match the version set in
`project/Project.scala`.

To deploy a snapshot version, pass the version to the deploy script:

```$bash
$ ./deploy v1.3.2-SNAPSHOT
Snapshot version:  1.3.0-SNAPSHOT
Deploying dropwizard-scala-core_2.11 snapshot....
(etc.)
```

To deploy a release version, check out the release tag and run the release 
script without arguments:

```$bash
$ git checkout v1.3.2
$ ./deploy
Getting release version from tag
Release version:  1.3.2
Deploying dropwizard-scala-core_2.11 release....
``` 
