### Deploying on Tanzu Application Service

Build React font-end assets locally. First, install the NPM packages needed to
build the React app:
```bash
npm install --dev`
```

Then build the React app:
```bash
npm run build
```

Finally, deploy the nginx server and React app using the `cf` CLI:
```bash
cf push -f manifest.yaml
```
