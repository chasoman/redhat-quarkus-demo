# Getting started with Quarkus

This is a minimal CRUD service exposing a couple of endpoints over REST.

Under the hood, this demo uses:

- RESTEasy to expose the REST endpoints
- REST-assured and JUnit 5 for endpoint testing

## Requirements

To compile and run this demo you will need:

- JDK 1.8+
- GraalVM

### Configuring GraalVM and JDK 1.8+

Make sure that both the `GRAALVM_HOME` and `JAVA_HOME` environment variables have
been set, and that a JDK 1.8+ `java` command is on the path.

See the [Building a Native Executable guide](https://quarkus.io/guides/building-native-image-guide)
for help setting up your environment.

## Building the application

Launch the Maven build on the checked out sources of this demo:

> ./mvnw install

### Live coding with Quarkus

The Maven Quarkus plugin provides a development mode that supports
live coding. To try this out:

> ./mvnw quarkus:dev

This command will leave Quarkus running in the foreground listening on port 8080.

1. Visit the default endpoint: [http://127.0.0.1:8080](http://127.0.0.1:8080).
    - Make a simple change to [src/main/resources/META-INF/resources/index.html](src/main/resources/META-INF/resources/index.html) file.
    - Refresh the browser to see the updated page.
2. Visit the `/hello` endpoint: [http://127.0.0.1:8080/hello](http://127.0.0.1:8080/hello)
    - Update the response in [src/main/java/org/acme/quickstart/GreetingResource.java](src/main/java/org/acme/quickstart/GreetingResource.java). Replace `hello` with `hello there` in the `hello()` method.
    - Refresh the browser. You should now see `hello there`.
    - Undo the change, so the method returns `hello` again.
    - Refresh the browser. You should now see `hello`.

### Run Quarkus in JVM mode

When you're done iterating in developer mode, you can run the application as a
conventional jar file.

First compile it:

> ./mvnw install

Then run it:

> java -jar ./target/quarkus-app/quarkus-run.jar

Have a look at how fast it boots, or measure the total native memory consumption.

### Run Quarkus as a native executable

You can also create a native executable from this application without making any
source code changes. A native executable removes the dependency on the JVM:
everything needed to run the application on the target platform is included in
the executable, allowing the application to run with minimal resource overhead.

Compiling a native executable takes a bit longer, as GraalVM performs additional
steps to remove unnecessary codepaths. Use the  `native` profile to compile a
native executable:

> ./mvnw install -Dnative

After getting a cup of coffee, you'll be able to run this executable directly:

> ./target/getting-started-1.0.0-SNAPSHOT-runner

### Deploy Quarkus to Red Hat Code Ready OpenShift Cluster
You can deploy the Quarkus app to the Red Hat Code Ready OpenShift Cluster by following these commands.
Take a look at this file - src/main/docker/Dockerfile.jvm

First login to the oc cluster

Then create a build:

> oc new-build --strategy docker --dockerfile - --code . --name getting-started < src/main/docker/Dockerfile.jvm

Next create a deployment:

> oc start-build --from-dir . getting-started

From the OpenShift console (in Admin mode) observe that an image stream should be created and note the name.

Now create an app from the image stream:

> oc new-app --image-stream <<Replace the image stream name in here>> --name getting-started

Finally create a route to expose the app to the world:

> oc expose service/getting-started
