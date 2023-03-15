## Tanzu Application Platform (TAP)
Using the `config/workload.yaml` it is possible to test, build and deploy this application onto a
Kubernetes cluster that is provisioned with Tanzu Application Platform (https://tanzu.vmware.com/application-platform).

Before deploying your application a Tekton Pipeline responsible for the testing step should be created in your application
namespace. To support [Pact](https://docs.pact.io/)-based consumer-driven contract testing Python and GCC should be available on an environment where
tests are executed. You need to create a Docker image which provides this kind of environment. You can use a Docker file
available in the `config/Dockerfile` to create one. Please go the `config` directory and execute use following commands.

> Be aware that you have to login to the image registry of your choice beforehand and you must have a write access to this registry.

> **Note:** The image that is created must be an `amd64` image so you can not build this on a system with an ARM processor like the newer Apple systems with M1 or M2 processors.

```bash
 docker build -t <your-image-registry.io>/<your-developer-namespace-project>/react-test-with-pact:node-19 - < Dockerfile
 docker push <your-image-registry.io>/<your-developer-namespace-project>/react-test-with-pact:node-19
```

Please ensure that an image which referenced in the pipeline definition (`config/react-test-pipeline.yaml` line 24) is the same
as the one you created on the previous step. To create a pipeline you can use following command.

```bash
kubectl apply -f config/react-test-pipeline.yaml
```

> One can have several pipelines available simultaneously. Matching of a pipeline with a workload is done based on a label assign to a pipeline.  
> Pipeline:
> ```
> apiVersion: tekton.dev/v1beta1
> kind: Pipeline
> metadata:
>   name: react-test-pipeline
>   labels:
>     apps.tanzu.vmware.com/pipeline: test-react
> ...
> ```  
> Workload:
> ```
> apiVersion: carto.run/v1alpha1
> kind: Workload
> ...
> spec:
>   params:
>     - name: testing_pipeline_matching_labels
>       value:
>         apps.tanzu.vmware.com/pipeline: test-react
> ``` 

### Tanzu CLI
Using the Tanzu CLI one could apply the workload using the local sources:
```bash
tanzu apps workload apply \
  --file config/workload.yaml \
  --namespace <namespace> --source-image <image-registry> \
  --local-path . \
  --yes \
  --tail
````

Note: change the namespace to where you would like to deploy this workload. Also define the (private) image registry you
are allowed to push the source-image, like: `docker.io/username/repository`.

### Visual Studio Code Tanzu Plugin
When developing local but would like to deploy the local code to the cluster the Tanzu Plugin could help.
By using `Tanzu: Apply` on the `workload.yaml` it will create the Workload resource with the local source (pushed to an image registry) as
starting point.

## Deployment topology
A running pod of this workload will include a built React application, NGINX server and NGINX configuration. A built React application
contains set of JavaScript, HTML, CSS and other static files which will be served by the NGINX HTTP server. Additionally to it the NGINX server
will act as a reverse proxy rerouting requests to the backend services. The rerouting rules you can find and adapt in the NGINX configuration
located in *nginx.conf* (see lines 161-163).  
![Deployment Topology](DeploymentTopology.png)

