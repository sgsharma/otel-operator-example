# Honeycomb Academy: Sample Meminator App

**_This is a demo app, don't run it in production_**

This contains a sample application for use in auto-instrumenting with the Opentelemetry Operator. This app has 4 services.

It generates images by combining a randomly chosen picture with a randomly chosen phrase.

## Running the application

### Kubernetes Quickstart

1. Launch Kubernetes cluster with Docker Desktop:

   - Launch Docker Desktop. Go to Preferences:
     - choose “Enable Kubernetes”,
     - set CPUs to at least 3, and Memory to at least 6.0 GiB
     - on the "Disk" tab, set at least 32 GB disk space

2. Make sure you have the correct context set:

```
kubectl config use-context docker-desktop
```

3. Run kubectl get nodes to verify you're connected to the respective control plane.

4. If you don't have a Honeycomb API key handy, here is the [documentation](https://docs.honeycomb.io/get-started/configure/environments/manage-api-keys/#create-api-key).

5. Add your Honeycomb API key as a secret from the command line. Replace $HONEYCOMB_API_KEY with your actual API key. For example, if your API key is abc123, run the following command:

```
export HONEYCOMB_API_KEY=abc123
kubectl create secret generic honeycomb --from-literal=api-key=$HONEYCOMB_API_KEY
```

6. Make sure you have cert-manager installed.

```
helm install cert-manager jetstack/cert-manager \
      --namespace default \
      --create-namespace \
      --version v1.15.3 \
      --set crds.enabled=true
```

7. Install the OpenTelemetry Collector Helm chart.

```
helm repo add open-telemetry https://open-telemetry.github.io/opentelemetry-helm-charts

# optionally run helm repo update

helm install opentelemetry-collector open-telemetry/opentelemetry-collector \
   --set image.repository="otel/opentelemetry-collector-k8s" \
 --values ./opentelemetry-collector-values-daemonset.yaml
```

8. Install the Opentelemetry Operator:

```
helm install \
    --set admissionWebhooks.certManager.enabled=false \
    --set admissionWebhooks.autoGenerateCert.enabled=true \
    --set manager.collectorImage.repository="otel/opentelemetry-collector-k8s" \
    --namespace honeycomb \
    --create-namespace \
      opentelemetry-operator open-telemetry/opentelemetry-operator
```

8. Apply autoinstrumentation to the Meminator app:

```
kubectl apply -n honeycomb -f ./otel-autoinstrumentation.yaml
```

### Run the app

Make sure you have skaffold installed. If not, install it from [here](https://skaffold.dev/docs/install/).

Then run:

```
skaffold run
```

Access the app:

[http://localhost:10114]()

### Try it out

Visit [http://localhost:10114]()

Click the "GO" button. Then wait.

View your traces in Honeycomb: [https://ui.honeycomb.io/](https://ui.honeycomb.io/)

### Stop the application

To stop the application, run:

```
skaffold delete
```
