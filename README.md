# datastore-emulator

A [Google Cloud Datastore Emulator](https://cloud.google.com/datastore/docs/tools/datastore-emulator/) container image.
The image is meant to be used for creating an standalone emulator for testing.

## Quickstart

To run the emulator in a standalone Docker container:

```bash
docker run --rm -it \
  --env "DATASTORE_LISTEN_ADDRESS=0.0.0.0:8081" \
  --env "DATASTORE_PROJECT_ID=test-project" \
  --publish 8081:8081 \
  quay.io/gumo/datastore-emulator
```

## Example docker-compose configuration

To run the emulator in a `docker-compose` configuration with your app, use the following example configuration for reference.

```YAML
version: "3"

services:
  datastore:
    image: quay.io/gumo/datastore-emulator
    environment:
      DATASTORE_PROJECT_ID: test-project
      DATASTORE_LISTEN_ADDRESS: 0.0.0.0:8081
    ports:
      - "8081:8081"
    volumes:
      - datastore-emulator-storage:/opt/data
    # command: start-datastore --no-store-on-disk --consistency=1.0

volumes:
  datastore-emulator-storage:
    driver: local
```

### Persistence

The image has a volume mounted at `/opt/data`. To maintain states between restarts, mount a volume at this location.

## Environment Variables

The following environment variables must be set:

- `DATASTORE_LISTEN_ADDRESS`: The address should refer to a listen address, meaning that `0.0.0.0` can be used. The address must use the syntax `HOST:PORT`, for example `0.0.0.0:8081`. The container must expose the port used by the Datastore emulator.
- `DATASTORE_PROJECT_ID`: The ID of the Google Cloud project for the emulator.

## Connect application with the emulator

The following environment variables need to be set so your application connects to the emulator instead of the production Cloud Datastore environment:

- `DATASTORE_EMULATOR_HOST`: The listen address used by the emulator.
- `DATASTORE_PROJECT_ID`: The ID of the Google Cloud project used by the emulator.

## Custom commands

This image contains a script named `start-datastore` (included in the PATH). This script is used to initialize the Datastore emulator.

### Starting an emulator

By default, the following command is called:

```sh
start-datastore
```
### Starting an emulator with options

This image comes with the following options: `--no-store-on-disk` and `--consistency`. Check [Datastore Emulator Start](https://cloud.google.com/sdk/gcloud/reference/beta/emulators/datastore/start). `--legacy`, `--data-dir` and `--host-port` are not supported by this image.

```sh
start-datastore --no-store-on-disk --consistency=1.0
```

## Reference

 - [Google Cloud SDK - Release Notes](https://cloud.google.com/sdk/docs/release-notes)
 - [Details of Bucket: gs://cloud-sdk-release - Google Cloud Platform](https://console.cloud.google.com/storage/browser/cloud-sdk-release?authuser=0)
