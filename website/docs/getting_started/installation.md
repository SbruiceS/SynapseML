---
title: Installation
description: Getting started with SynapseML
---

## Synapse

SynapseML can be conveniently installed on Synapse:

For Spark3.1 pool:
```python
%%configure -f
{
  "name": "synapseml",
  "conf": {
      "spark.jars.packages": "com.microsoft.azure:synapseml_2.12:0.9.5-13-d1b51517-SNAPSHOT",
      "spark.jars.repositories": "https://mmlspark.azureedge.net/maven",
      "spark.jars.excludes": "org.scala-lang:scala-reflect,org.apache.spark:spark-tags_2.12,org.scalactic:scalactic_2.12,org.scalatest:scalatest_2.12",
      "spark.yarn.user.classpath.first": "true"
  }
}
```

For Spark3.2 pool:
```python
%%configure -f
{
  "name": "synapseml",
  "conf": {
      "spark.jars.packages": "com.microsoft.azure:synapseml_2.12:0.10.1",
      "spark.jars.repositories": "https://mmlspark.azureedge.net/maven",
      "spark.jars.excludes": "org.scala-lang:scala-reflect,org.apache.spark:spark-tags_2.12,org.scalactic:scalactic_2.12,org.scalatest:scalatest_2.12,com.fasterxml.jackson.core:jackson-databind",
      "spark.yarn.user.classpath.first": "true"
  }
}
```

## Python

To try out SynapseML on a Python (or Conda) installation you can get Spark
installed via pip with `pip install pyspark`.  You can then use `pyspark` as in
the above example, or from python:

```python
import pyspark
spark = pyspark.sql.SparkSession.builder.appName("MyApp") \
            # Please use 0.10.1 version for Spark3.2 and 0.9.5-13-d1b51517-SNAPSHOT version for Spark3.1
            .config("spark.jars.packages", "com.microsoft.azure:synapseml_2.12:0.10.1") \
            .config("spark.jars.repositories", "https://mmlspark.azureedge.net/maven") \
            .getOrCreate()
import synapse.ml
```

## SBT

If you are building a Spark application in Scala, add the following lines to
your `build.sbt`:

```scala
resolvers += "SynapseML" at "https://mmlspark.azureedge.net/maven"
// Please use 0.10.1 version for Spark3.2 and 0.9.5-13-d1b51517-SNAPSHOT version for Spark3.1
libraryDependencies += "com.microsoft.azure" %% "synapseml_2.12" % "0.10.1"

```

## Spark package

SynapseML can be conveniently installed on existing Spark clusters via the
`--packages` option, examples:

```bash
# Please use 0.10.1 version for Spark3.2 and 0.9.5-13-d1b51517-SNAPSHOT version for Spark3.1
spark-shell --packages com.microsoft.azure:synapseml_2.12:0.10.1
pyspark --packages com.microsoft.azure:synapseml_2.12:0.10.1
spark-submit --packages com.microsoft.azure:synapseml_2.12:0.10.1 MyApp.jar
```

This can be used in other Spark contexts too. For example, you can use SynapseML
in [AZTK](https://github.com/Azure/aztk/) by [adding it to the
`.aztk/spark-defaults.conf`
file](https://github.com/Azure/aztk/wiki/PySpark-on-Azure-with-AZTK#optional-set-up-mmlspark).

## Databricks

To install SynapseML on the [Databricks
cloud](http://community.cloud.databricks.com), create a new [library from Maven
coordinates](https://docs.databricks.com/user-guide/libraries.html#libraries-from-maven-pypi-or-spark-packages)
in your workspace.

For the coordinates use: `com.microsoft.azure:synapseml_2.12:0.10.1` for Spark3.2 Cluster and
 `com.microsoft.azure:synapseml_2.12:0.9.5-13-d1b51517-SNAPSHOT` for Spark3.1 Cluster;
Add the resolver: `https://mmlspark.azureedge.net/maven`. Ensure this library is
attached to your target cluster(s).

Finally, ensure that your Spark cluster has at least Spark 3.12 and Scala 2.12.

You can use SynapseML in both your Scala and PySpark notebooks. To get started with our example notebooks import the following databricks archive:

`https://mmlspark.blob.core.windows.net/dbcs/SynapseMLExamplesv0.10.1.dbc`

## Apache Livy and HDInsight

To install SynapseML from within a Jupyter notebook served by Apache Livy the following configure magic can be used. You will need to start a new session after this configure cell is executed.

Excluding certain packages from the library may be necessary due to current issues with Livy 0.5

```
%%configure -f
{
    "name": "synapseml",
    "conf": {
        # Please use 0.10.1 version for Spark3.2 and 0.9.5-13-d1b51517-SNAPSHOT version for Spark3.1
        "spark.jars.packages": "com.microsoft.azure:synapseml_2.12:0.10.1",
        "spark.jars.excludes": "org.scala-lang:scala-reflect,org.apache.spark:spark-tags_2.12,org.scalactic:scalactic_2.12,org.scalatest:scalatest_2.12,com.fasterxml.jackson.core:jackson-databind"
    }
}
```

In Azure Synapse, "spark.yarn.user.classpath.first" should be set to "true" to override the existing SynapseML packages

```
%%configure -f
{
    "name": "synapseml",
    "conf": {
        # Please use 0.10.1 version for Spark3.2 and 0.9.5-13-d1b51517-SNAPSHOT version for Spark3.1
        "spark.jars.packages": "com.microsoft.azure:synapseml_2.12:0.10.1",
        "spark.jars.excludes": "org.scala-lang:scala-reflect,org.apache.spark:spark-tags_2.12,org.scalactic:scalactic_2.12,org.scalatest:scalatest_2.12,com.fasterxml.jackson.core:jackson-databind",
        "spark.yarn.user.classpath.first": "true"
    }
}
```

## Docker

The easiest way to evaluate SynapseML is via our pre-built Docker container.  To
do so, run the following command:

```bash
docker run -it -p 8888:8888 -e ACCEPT_EULA=yes mcr.microsoft.com/mmlspark/release
```

Navigate to <http://localhost:8888/> in your web browser to run the sample
notebooks.  See the [documentation](reference/docker.md) for more on Docker use.

> To read the EULA for using the docker image, run
``` bash
docker run -it -p 8888:8888 mcr.microsoft.com/mmlspark/release eula
```


## Building from source

SynapseML has recently transitioned to a new build infrastructure.
For detailed developer docs please see the [Developer Readme](reference/developer-readme.md)

If you are an existing SynapseML developer, you will need to reconfigure your
development setup. We now support platform independent development and
better integrate with intellij and SBT.
 If you encounter issues please reach out to our support email!

## R (Beta)

To try out SynapseML using the R autogenerated wrappers [see our
instructions](reference/R-setup.md).  Note: This feature is still under development
and some necessary custom wrappers may be missing.

## C# (.NET)

To try out SynapseML with .NET, please follow the [.NET Installation Guide](reference/dotnet-setup.md).
Note: Some stages including AzureSearchWriter, DiagnosticInfo, UDPyF Param, ParamSpaceParam, BallTreeParam,
ConditionalBallTreeParam, LightGBMBooster Param are still under development and not exposed in .NET.
