# Honeycomb Academy: Sample Meminator App

**_This is a demo app, don't run it in production_**

This contains a sample application for use in Honeycomb Academy lab activities. This app has 4 services.

It generates images by combining a randomly chosen picture with a randomly chosen phrase.

## Introduction

Hello! Welcome to the **Instrumenting with Python** course lab.

1. Take a look at this app. The `backend-for-frontend` service needs to be instrumented.
2. Before you can do that, you need to run this app.
3. Then, connect this app to Honeycomb.
4. See what the traces look like.
5. Improve the traces.

## Running the application

To run this app, you can use GitPod or Codespaces.

Once you run the application, you can send traces to Honeycomb. Then you can practice improving the instrumentation for better observability.

### GitPod setup

Go to [Gitpod](https://gitpod.io/#https://github.com/honeycombio/academy-instrumentation-python) to launch this project in Gitpod.

Confirm the workspace creation. You can work in the browser with VS Code Browser or in your local code editor. The default settings are acceptable.

Once you are in the code editor, run `docker compose up` in the code editor's terminal. To stop running the application, run `ctrl+c`. Then run `docker compose down` to remove the container.

### Codespaces setup

Open the repository on GitHub. Open the `<> Code` dropdown down menu.

Select the `Codespaces` tab. Create a codespace on main.

### Local development setup

You also have the option to run this application locally.

First, clone this repository.

```bash
git clone https://github.com/honeycombio/academy-instrumentation-python.git
```

Install Docker: https://docs.docker.com/get-docker/

Create a `.env` file from the example:

```bash
cp example.env .env
```

And update the `.env` file with your Honeycomb API key:

```bash
HONEYCOMB_API_KEY="your-api-key"

# you could change this to your own S3 bucket of images. We accept no responsibility for the outcome.
# Note: "random-pictures" is an actual S3 bucket name supplied for this course, filled with SFW meme images
BUCKET_NAME="random-pictures"

OTEL_EXPORTER_OTLP_ENDPOINT="https://api.honeycomb.io:443/"
OTEL_EXPORTER_OTLP_HEADERS="x-honeycomb-team=${HONEYCOMB_API_KEY}"

# You can set this to "services-implemented-version" to run the answer-guide version
SERVICE_PATH="services"
```

If you don't have an API key handy, here is the [documentation](https://docs.honeycomb.io/get-started/configure/environments/manage-api-keys/#create-api-key).

### Run the app

`./run`

(This will run `docker compose` in daemon mode, and build containers.)

Access the app:

[http://localhost:10114]()

After making changes to a service, you can tell it to rebuild just that one:

`./run [ meminator | backend-for-frontend | image-picker | phrase-picker ]`

### Try it out

Visit [http://localhost:10114]()

Click the "GO" button. Then wait.
