# Sample Node App

This sample app demonstrates read operations on a GDN KV collection using Local DB URL.

## Prerequisites

1. Install [Docker Desktop](https://docs.docker.com/get-docker/).
2. Setup [Docker hub account](https://hub.docker.com/) to manage the container images.
3. Install [Node.js 14](https://nodejs.org/en/download/).
    ```
    # Using Homebrew
    $ brew install node@14
    ```
4. Create `Key-Value Collection` in your Macrometa GDN account/tenant.

## Build Docker container

-   Use the following commands to build and push the container image to Docker hub. In below commands please update `macrometacorp/sample-node-app` with your docker repository name.

    ```
    $ git clone https://github.com/Macrometacorp/compute-samples.git
    cd compute-samples/sample-node-app
    docker build -t macrometalabs/sample-node-app . --platform=linux/amd64 --no-cache
    docker push macrometalabs/sample-node-app:latest
    ```

## Setup Macrometa Command Line Interface (CLI)

1. Install Command Line Interface (CLI)

    ```
    $ npm install -g gdnsl
    ```

2. Create an API key in the Macrometa GDN using it's GUI. This API key is use to deploy serverless microservices using cli and make api request to get data from KV collection.

3. Initialize CLI
    ```
    $ gdnsl init
    Please enter GDN URL.: https://gdn.paas.macrometa.io
    Please enter the email.: demo@demo.com [Please enter email used for GDN account]
    Please enter apiKey.: xxxx [Please enter API key created in steup 2]
    Getting regions details...
    Available regions are:
            gdn-eu-central,gdn-us-west,gdn-ap-south
            Otherwise you can either use LOCAL or ALL also
    Please enter the name of the regions. For multiple regions enter comma-separated names.
    For example: region1, region2.: gdn-us-west,gdn-ap-south
    ```

## Deploy serverless microservices using CLI

Now, we are ready to deploy our sample node app on Macrometa Compute environment.

```
gdnsl service create <service_name> \
    --env URL_LOCAL_DB=c8db-coord-svc.c8.svc.cluster.local:8529 \
    --env COLLECTION_KV_NAME=sample_kv \
    --env API_KEY=xxxx\
    --image macrometacorp/sample-node-app:latest --scale-min 1
```

## CLI commands:

1. List Available services on Compute

    ```
    $ gdnsl service list
    ```

2. Show details for a service

    ```
    $ gdnsl service describe <service_name>
    ```

3. Execute service endpoint using curl

    ```
    $ curl -X 'GET' <URL> --insecure
    ```

4. Update service configuration

    ```
    $ gdnsl service update <service_name>
    ```

5. Delete a service.

    ```
    $ gdnsl service delete <service_name>
    ```

## References

-   [Macrometa CLI](https://macrometa.com/docs/essentials/CLI/overview)
-   [Macrometa CLI Service](https://macrometa.com/docs/essentials/CLI/commands#service-gdnsl-service)
