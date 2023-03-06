# Deploying on Tanzu Application Service

From `react-frontend/` source directory (NOT output of accelerator):
1. Modify `nginx.conf` to include your backend URL for `location /api/`
1. `❯ npm install --dev`
1. `❯ npm run build`
1. `❯ cf push -f ./vms/tas/manifest.yaml`
