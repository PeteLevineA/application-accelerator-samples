### Deploying on Tanzu Application Service

1. `mvn clean package`
1. `cf push -p ./target/hello-fun-0.0.1-SNAPSHOT.jar -f manifest.yaml`
