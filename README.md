# Ollama Pipeline for Lilypad and Docker 🐋
Based on Ollama, the Ollama Pipeline modules for Lilypad allow you generate text on Lilypad using various models.
docker build -t arsen3d/ollama:llama3-8b-lilypad1 -f Dockerfile-llama3-8b https://github.com/arsen3d/lilypad-module-ollama-pipeline.git#test
docker push arsen3d/ollama:llama3-8b-lilypad1
[WORK IN PROGRESS, HERE BE DRAGONS]

# Usage
These modules are designed to be run in a Docker container, either through the Lilypad Network or in Docker directly.

## Options and tunables
The following tunables are available. All of them are optional, and have default settings that will be used if you do not provide them.

| Name | Description | Default | Available options |
|------|-------------|---------|-------------------|
| `Prompt` | A text prompt for the model | "question mark floating in space" | Any string |

See the usage sections for the runner of your choice for more information on how to set and use these variables.

## Lilypad
To run Ollama Pipeline in Lilypad, you can use the following commands:

### LLaMa3
```bash
lilypad run ollama-pipeline:llama3-8b-lilypad1 -i Prompt='Something awesome this way comes'
```

### Specifying tunables

If you wish to specify more than one tunable, such as the number of steps, simply add more `-i` flags, like so:

```bash
lilypad run sdxl-pipeline -i Prompt="an astronaut floating against a white background" -i Steps=69
```

See the options and tunables section for more information on what tunables are available.

## Docker

To run these modules in Docker, you can use the following commands:

### LLaMa3

```bash
docker run -ti --gpus all \
    -v $PWD/outputs:/outputs \
    -e PROMPT="an astronaut floating against a white background" \
    zorlin/ollama:llama3-8b-lilypad1
```

### Specifying tunables
If you wish to specify more than one tunable, such as the number of steps, simply add more `-e` flags, like so:

```bash
-e PROMPT="an astronaut floating against a white background" \
-e STEPS=69 \
-e SIZE=2048 \
```

See the options and tunables section for more information on what tunables are available.

# Development
You can build the Docker containers that form this module by following these steps (replacing Dockerfile-llama3-8b and its Git tags with the appropriate Dockerfile and tags for the model you wish to use):

```
# From the root directory of this repository, change to the docker folder.
cd docker
# Build the docker image
DOCKER_BUILDKIT=1 docker build -f Dockerfile-llama3-8b -t zorlin/ollama:llama3-8b-lilypad1 --target runner .
```
```
mkdir -p outputs
```

# Publishing for production
To publish all of the images, run `scripts/build.sh`. Once you're satisfied, run `release.sh`.

# Testing
Fork this repository and make your changes. Then, build a Docker container and run the module with your changes locally to test them out.

Once you've made your changes, publish your Docker image, then edit `lilypad_module.json.tmpl` to point at it and create a Git tag such as `v0.9-lilypad10`.

You can then run your module with 

`lilypad run github.com/zorlin/example:v0.1.2 -i Prompt="A very awesome dragon riding a unicorn"` to test your changes, replacing `zorlin` with your username and `v0.1.2` with the tag you created.

Note that most nodes on the public Lilypad network will be unlikely to run your module (due to allowlisting), so you may need to run a Lilypad node to test your changes. Once your module is stable and tested, you can request that it be adopted as an official module. Alternatively, if you're simply making changes to this module instead of making a new one, feel free to submit a pull request.

# Credits
Authored by the Lilypad team. Maintained by [Zorlin](https://github.com/Zorlin).

Based on [sdxl-pipeline](https://github.com/lilypad-tech/lilypad-module-sdxl-pipeline), which was written by early Lilypad contributors.

Based on Ollama.
