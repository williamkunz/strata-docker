# Strata for Unraid

> [!WARNING]
> **Installed Strata before this change? Switch it to the official image.** The template now uses Strata's official image, `ghcr.io/niko1221/strata:latest-cuda13`, and I no longer build or update my own image (`ghcr.io/williamkunz/strata`). An existing install keeps its saved settings, so it stays on my old image and stops getting updates. Either:
>
> - **Reinstall (recommended):** remove the container and install Strata again from the Apps tab, using the same **Data** path. You get the new template's settings.
> - **Or edit the container:** set **Repository** to `ghcr.io/niko1221/strata:latest-cuda13` and, under Advanced View, **Extra Parameters** to `--runtime=nvidia --ulimit memlock=-1:-1 --shm-size=2g --stop-timeout 120`, then Apply. The old Reinstall and Low-RAM settings are ignored (the log warns about them), and the new settings use their defaults.
>
> Either way the downloaded model files in the Data folder are reused, so nothing large is downloaded again.

An Unraid Community Applications template for [Strata](https://github.com/Niko1221/Strata), which runs the Qwen3.8-Flash-Next mixture-of-experts model on an NVIDIA GPU plus system RAM. It serves an OpenAI- and Anthropic-compatible API, a chat page and a live monitor.

The template runs Strata's official image, `ghcr.io/niko1221/strata:latest-cuda13`. Report template or Unraid problems in this repository's [issues](https://github.com/williamkunz/strata-docker/issues), and problems with Strata itself to the [Strata project](https://github.com/Niko1221/Strata/issues).

## Install

Search for **Strata** in the Apps tab. Before installing you need:

- the **Nvidia Driver plugin** on driver 580 or newer, and an RTX 20/30/40/50 card. For a GTX 10, P100 or V100, or a driver from 575 to 579, change the Repository tag to `latest-cuda12`.
- 32 GB of RAM or more; more RAM allows a larger, better model ([Strata's model guide](https://github.com/Niko1221/Strata/blob/main/docs/MODELS.md))
- room for the model on an SSD pool, mapped as a direct pool path (`/mnt/cache/...`), not `/mnt/user`

The first start downloads the model and prepares it. Later starts go straight to serving. The template's Overview explains each setting, and Strata's [Docker guide](https://github.com/Niko1221/Strata/blob/main/docs/DOCKER.md) covers them in full.

An API key is always on. If you leave it empty, the first start makes one; show it with:

```bash
docker exec Strata strata launch --show-key --preset docker
```

## Updates

The template follows `latest-cuda13`, which Strata's maintainer moves to each new release when its image is published. Unraid shows the update on the Docker tab. To stay on one release, change the tag to a version such as `0.2.3-cuda13`.

## Credits and licence

[Strata](https://github.com/Niko1221/Strata) and its image are by Niko1221 and the Strata contributors, under the MIT licence. The icon is Strata's own. This repository is MIT licensed; see [LICENSE](LICENSE).
