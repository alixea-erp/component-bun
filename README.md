<img alt="icon" src=".alixea/icon.svg" width="32">

# Bun component for Alixea

A generic Bun component that can be used to run any JS / TS app. Uses the official
[oven/bun](https://hub.docker.com/r/oven/bun) Docker image.

Versions are tags: `v1.3.8` runs the app as a StatefulSet in development, `main` as a Deployment in every stage.

The prebuilt images (`prebuildImage` in `alixea.yaml`) are still the ones Diploi's CI pushes to its public ECR, built
from the same files; this repository has no image pipeline of its own yet.

## Operation

### Development

Runs `bun install` when the component is first initialized, and `bun dev` when the deployment is started.

### Production

Builds a production ready image, which runs `bun install` and `bun run build` when it is created. Once the image runs,
`bun start` is called.

## Links

- [Bun documentation](https://bun.sh/docs)
