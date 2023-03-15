### Deploying on Tanzu Application Service

Build a JAR file locally using Maven:
```bash
mvn clean package
```

Deploy the JAR on TAS using the `cf` CLI:
```bash
cf push -p ./target/hello-fun-0.0.1-SNAPSHOT.jar -f manifest.yaml`
```
