Metrics-Scala
=============

*Capturing JVM- and application-level metrics. So you know what's going on.*

This is the Scala API for [Dropwizard's Metrics](https://github.com/dropwizard/metrics) library.

Initially this project started out as a line for line copy of the Metrics-scala module, released for multiple
scala versions. Metrics dropped the scala module in version 3.0.0 and this project continued separately
with the help of [@scullxbones](https://github.com/scullxbones) and many other contributors.

We strive for long term stability, correctness, an easy to use API and full documentation (in that order).

### Contents

* Usage
* [Manual](/docs/Manual.md)
* [Manual (version 2.x)](/docs/Manual_2x.md)
* Features
* Available artifacts
* Download
* Support
* License
* [![Scaladocs](https://www.javadoc.io/badge/nl.grons/metrics4-scala_2.12.svg?color=brightgreen&label=Scaladocs)](https://www.javadoc.io/page/nl.grons/metrics4-scala_2.12/latest/nl/grons/metrics4/scala/DefaultInstrumented.html)
  [![Scaladocs](https://img.shields.io/badge/javadoc-3.5.9-brightgreen.svg?color=brightgreen&label=Scaladocs)](https://www.javadoc.io/page/nl.grons/metrics-scala_2.12/3.5.9_a2.4/nl/grons/metrics/scala/DefaultInstrumented.html)
* Travis: [![build status](https://travis-ci.org/erikvanoosten/metrics-scala.svg?branch=master)](https://travis-ci.org/erikvanoosten/metrics-scala)

### Usage

Metrics-scala provides an easy way to create _metrics_ and _health checks_ in Scala. Since version 3.5.5 creating
metrics and health checks is as easy as extending
[DefaultInstrumented](/metrics-scala/src/main/scala/nl/grons/metrics4/scala/DefaultInstrumented.scala) and using the
`metrics` and `healthCheck` builders:

```scala
class Example(db: Database) extends nl.grons.metrics4.scala.DefaultInstrumented {
  // Define a health check
  healthCheck("alive") { workerThreadIsActive() }

  // Define a timer metric
  private[this] val loading = metrics.timer("loading")

  // Use timer metric
  def loadStuff(): Seq[Row] = loading.time {
    db.fetchRows()
  }
}
```

For more detailed information see the [manual](/docs/Manual.md). For more information on Dropwizard-metrics 4.x, please
see the [documentation](http://metrics.dropwizard.io/4.0.0/).

See also the [change log](CHANGELOG.md) for improvements and API changes.

### Features

* Easy creation of all metrics types.
* Easy creation of Health Checks.
* Almost invisible syntax for using timers (see example above).
* Scala specific methods on metrics (e.g. `+=` on counters).
* Derives proper metrics names for Scala objects and closures.
* Actor support.
* Future support.
* [Hdrhistogram](http://hdrhistogram.org/) support.

## Available artifacts (abbreviated)

The following artifacts are available:

* *metrics4-scala*: adds a nice Scala API to Dropwizard Metrics
* *metrics4-akka*: support for measuring Akka actors
* *metrics4-hdr*: adds support for [HdrHistogram](http://www.hdrhistogram.org/) to increase the accuracy of histograms 

The table shows the available artifacts of metrics-scala. For the full list, including those targeting older Scala and
Akka versions see [all available versions](/docs/AvailableVersions.md).

<table border="0" cellpadding="2" cellspacing="2">
  <tbody>
    <tr>
      <td valign="top" rowspan="2">Artifact name</td>
      <td valign="top" rowspan="1" colspan="3">Scala version</td>
      <td valign="top" rowspan="1" colspan="2">Akka version</td>
      <td valign="top" rowspan="2">Build against</td>
    </tr>
    <tr>
      <td valign="top">2.11</td>
      <td valign="top">2.12</td>
      <td valign="top">2.13</td>
      <td valign="top">2.5</td>
      <td valign="top">2.4</td>
    </tr>
    <tr>
      <td valign="top">metrics4-scala</td>
      <td valign="top">✓</td>
      <td valign="top">✓</td>
      <td valign="top">✓</td>
      <td valign="top"></td>
      <td valign="top"></td>
      <td valign="top">Dropwizard-metrics 4.1.1</td>
    </tr>
    <tr>
      <td valign="top">metrics4-akka_a25</td>
      <td valign="top"></td>
      <td valign="top">✓</td>
      <td valign="top">✓</td>
      <td valign="top">✓</td>
      <td valign="top"></td>
      <td valign="top">Akka 2.5.26</td>
    </tr>
    <tr>
      <td valign="top">metrics4-akka_a24</td>
      <td valign="top">✓</td>
      <td valign="top">✓</td>
      <td valign="top"></td>
      <td valign="top">✓</td>
      <td valign="top">✓</td>
      <td valign="top">Akka 2.4.20</td>
    </tr>
    <tr>
      <td valign="top">metrics4-scala-hdr</td>
      <td valign="top">✓</td>
      <td valign="top">✓</td>
      <td valign="top">✓</td>
      <td valign="top"></td>
      <td valign="top"></td>
      <td valign="top">Hdr 1.1.0/2.1.11 (**)</td>
    </tr>
  </tbody>
</table>

(*) RC versions are only supported until the next version is available.

(**) The first number is the version of `"org.mpierce.metrics.reservoir" % "hdrhistogram-metrics-reservoir"`,
the second the version of `"org.hdrhistogram" % "HdrHistogram"`.
See also [hdrhistogram manual page](/docs/Hdrhistogram.md).

## Migrating from 3.x to 4.x

Migrating from 3.x to 4.x is simply a matter of replacing the package from `nl.grons.metrics` to `nl.grons.metrics4`,
and recompiling the code.

Metrics-scala 3.x and metrics-scala 4.x can be used at the same time on top of either Dropwizard Metrics 3.x or 4.x
(excluding Dropwizard metrics 4.0.0). Metrics4-scala-hdr can only be used on top of Dropwizard Metrics 4.x.

## Download 4.x

<a href="CHANGELOG.md#v411-oct-2019">Release notes for 4.1.1.</a>

WARNING: `nl.grons:metrics-scala:4.0.0` was accidentally released as well. *Do not use it* as it will give
binary compatibility problems. Instead use `"nl.grons" %% "metrics4-scala" % "4.0.1"` or later as described below.

SBT:
```
libraryDependencies ++= Seq(
  "nl.grons" %% "metrics4-scala" % "4.1.1",
  "nl.grons" %% "metrics4-akka_a24" % "4.1.1",
  "nl.grons" %% "metrics4-scala-hdr" % "4.1.1"
)
```

Maven:
```
<properties>
    <scala.version>2.13.0</scala.version>
    <scala.compat.version>2.13</scala.compat.version>
    <metrics.scala.version>4.1.1</metrics.scala.version>
</properties>
<dependency>
    <groupId>nl.grons</groupId>
    <artifactId>metrics4-scala_${scala.compat.version}</artifactId>
    <version>${metrics.scala.version}</version>
</dependency>
<dependency>
    <groupId>nl.grons</groupId>
    <artifactId>metrics4-akka_a25_${scala.compat.version}</artifactId>
    <version>${metrics.scala.version}</version>
</dependency>
<dependency>
    <groupId>nl.grons</groupId>
    <artifactId>metrics4-scala-hdr_${scala.compat.version}</artifactId>
    <version>${metrics.scala.version}</version>
</dependency>
```

## Download 2.x and 3.x

In the 2.x and 3.x versions you depend on a single artifact. Different versions target different Akka/Scala
combinations. See [all available versions](/docs/AvailableVersions.md) to select the appropriate version.

Here are examples for metrics-scala 3.5.10 and Akka 2.4 (and Scala 2.12 in the Maven example):

SBT:
```
libraryDependencies += "nl.grons" %% "metrics-scala" % "3.5.10_a2.4"
```

Maven:
```
<properties>
    <scala.version>2.12.7</scala.version>
    <scala.compat.version>2.12</scala.compat.version>
</properties>
<dependency>
    <groupId>nl.grons</groupId>
    <artifactId>metrics-scala_${scala.compat.version}</artifactId>
    <version>3.5.10_a2.4</version>
</dependency>
```

To use hdrhistogram additional dependencies are needed. See the [hdrhistogram manual page](/docs/Hdrhistogram.md).

## Dropwizard 5.x

Dropwizard metrics 5.x has been in RC state for some time now. Metrics-scala support for it can be
found in the `metrics5-dev` branch. Since the master branch has since evolved considerably, the 5.x
branch is not in a releasable state anymore.

## Support

If you find a bug, please open an [issue](https://github.com/erikvanoosten/metrics-scala/issues), better yet: send a
pull request. For questions, please sent an email to the
[metrics mailing list](http://groups.google.com/group/metrics-user).

### License

Copyright (c) 2010-2012 Coda Hale, Yammer.com (before 3.0.0)

Copyright (c) 2013-2019 Erik van Oosten (3.0.0 and later)

Published under Apache Software License 2.0, see [LICENSE](LICENSE)
