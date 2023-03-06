# Deploying on Tanzu Application Service

From `spring-cloud-serverless/` source directory (NOT output of accelerator):
1. `❯ mvn clean package`
1. `❯ cf push -p ./target/hello-fun-0.0.1-SNAPSHOT.jar -f ./vms/tas/manifest.yaml`
