<img alt="icon" src=".alixea/icon.svg" width="32">

# Bun component for Alixea

A generic Bun component that can be used to run any JS / TS app. Runs on Alixea's Node.js base image
([component-bases](https://github.com/alixea-erp/component-bases)), which has Bun. The version tag is the Bun version
of the image.

## Operation

### Development

Installs the dependencies when the component starts, then runs `bun run dev`, which is `bun --watch src/index.ts`
and reloads on every change. This can be changed with the `containerCommands.developmentStart` field in `alixea.yaml`.

### Staging and production

Staging and production build the deployment's commit on the deployment itself, with the deployment's environment
variables: `bun install`, then `bun run build`. The template's build only prints a line, so put a build step there if
the app needs one. A commit is built once; a restart of the component reuses the build. A failed build leaves the
previous version running, and its log is shown on the deployment page, where the commit can also be built again.

The component then runs `bun run start`, which can be changed with the `containerCommands.productionStart` field in
`alixea.yaml`.

The project folder is read-only while the app runs, so it cannot write files next to its code. Keep data in a database
or another component, and use `/tmp` for a file needed only during one request.

### System packages

The image has Node.js, Bun and pnpm. Install more with `initScript` in `alixea.yaml`, which runs as root when the
component starts, or `preBuildScript`, which runs only before the build:

```yaml
- name: Bun
  identifier: bun
  package: https://github.com/alixea-erp/component-bun#v1.4.5
  initScript: |
    apt-get update && apt-get install -y --no-install-recommends imagemagick
```

### Ports

The component serves on port **3000**. To use another one, change the `port` of its `app` host in `alixea.yaml`.
Alixea passes that port to the app as `PORT`, and `HOSTNAME` is `0.0.0.0`. A start command of your own should listen
on `$PORT` too: staging and production count a new version as ready once it accepts connections on that port.

## Links

- [Bun documentation](https://bun.sh/docs)
