# Strata for Unraid

An Unraid Community Applications template for [Strata](https://github.com/Niko1221/Strata), which runs large mixture-of-experts models on an NVIDIA GPU plus system RAM: Qwen3.8-Flash-Next and its variants, the smaller Qwen3.6-35B-A3B, and DeepSeek-V4.1-Flash. It serves an OpenAI- and Anthropic-compatible API, a chat page and a live monitor.

The template runs Strata's official image, `ghcr.io/niko1221/strata:latest-cuda13`. Report template or Unraid problems in this repository's [issues](https://github.com/williamkunz/strata-docker/issues), and problems with Strata itself to the [Strata project](https://github.com/Niko1221/Strata/issues).

## Install

Search for **Strata** in the Apps tab. Before installing you need:

- the **Nvidia Driver plugin** on driver 580 or newer, and an RTX 20/30/40/50 card. For a GTX 10, P100 or V100, or a driver from 575 to 579, change the Repository tag to `latest-cuda12`.
- 32 GB of RAM or more; more RAM allows a larger, better model ([Strata's model guide](https://github.com/Niko1221/Strata/blob/main/docs/MODELS.md))
- room for the model on an SSD pool, mapped as a direct pool path (`/mnt/cache/...`), not `/mnt/user`

### Choosing a model

Leave **Model** and **Model family** empty and Strata picks one for your server; the log says which and why. To choose yourself, set both.

The model names are compression levels. The number is roughly the bits kept per weight: a smaller number means a smaller download and less RAM, at some cost in quality. RAM in the tables is the server's total RAM.

**Qwen3.8-Flash-Next**, the main model, for chat, code and tools. Model family `qwen`:

| Model | What it is | Download | RAM |
| --- | --- | ---: | ---: |
| `Q2_0` | the fastest, good quality | 66 GB | 48 GB |
| `IQ2_XS` | a little better, nearly as fast | 68 GB | 48 GB |
| `IQ3_XXS` | great quality, slower | 76 GB | 60 GB |
| `IQ3_S` | the best quality, matches the full model; the slowest | 84 GB | 64 GB |

**Other versions of Qwen3.8:**

| Model family | Model | What it is | Download | RAM |
| --- | --- | --- | ---: | ---: |
| `swift` | the four `qwen` sizes | UkisAI's fine-tune that thinks for less time before it answers | as `qwen` | as `qwen` |
| `coder` | `IQ1_M` | for code only, with half the model removed | 58 GB | 32 GB |
| `gyro` | `Gyro-S` | the least memory, lower quality | 59 GB | about 28 GB of RAM and graphics memory together |
| `gyro` | `Gyro-M` | about `IQ3_XXS` quality in less memory, 16–21% slower than `Q2_0` | 92 GB | about 35 GB together |
| `unsloth` | `UD-IQ4_XS` | 4-bit, better than `IQ3_S` | 94 GB | 72 GB |
| `unsloth` | `UD-Q4_K_XL` | 4-bit | 111 GB | 88 GB |
| `unsloth` | `UD-Q5_K_XL` | 5-bit, close to `Q8_0` | 158 GB | 108 GB |
| `unsloth` | `UD-Q6_K_XL` | 6-bit | 169 GB | 118 GB |
| `unsloth` | `Q8_0` | 8-bit, the reference quality | 188 GB | 136 GB |
| `nvfp4` | `NVFP4`, `NVFP4-NVIDIA`, `NVFP4-QAD` | NVIDIA's 4-bit format, fastest on RTX 50 cards; `NVFP4` has the best quality of the three | 102–128 GB | 80 GB |

The `unsloth` RAM figures keep the whole model in memory. With less RAM they still run, reading part of the model from the SSD, which is much slower.

**Qwen3.6-35B-A3B**, a smaller model for servers with less RAM. Model family `qwen36`:

| Model | What it is | Download | RAM |
| --- | --- | ---: | ---: |
| `UD-Q4_K_XL` | 4-bit, the smallest | 23 GB | 32 GB |
| `UD-Q6_K_XL` | 6-bit, closer to the full model | 33 GB | 44 GB |
| `Q8_0` | 8-bit, the closest to the full model | 38 GB | 52 GB |

**DeepSeek-V4.1-Flash**, the largest model. Model family `deepseek41`. Each download includes a 111 GB lookup table and an 8 GB helper model:

| Model | What it is | Download |
| --- | --- | ---: |
| `ist-das-2.25bpw` | the best quality, the best for code and agents | 274 GB |
| `ist-das-2.00bpw` | the middle choice | 257 GB |
| `ist-das-1.75bpw` | the smallest, noticeably weaker at writing prose | 239 GB |
| `ista-das-coder` | for code only, with half the model removed | 191 GB |

DeepSeek needs:

- the `latest-cuda13` tag (DeepSeek's engine has no CUDA 12 build) and an RTX 30/40/50 card with 12 GB or more
- 46 GB of RAM or more; 64 GB is the size Strata tested
- an NVMe pool for the Data folder: the model reads its lookup table from the disk as it writes

Expect about 5–10 tokens per second on a 12 GB card. DeepSeek uses its own engine, which the first start downloads into the Data folder. It is checked at `8k` context and offers up to `1m`. Strata's [model guide](https://github.com/Niko1221/Strata/blob/main/docs/MODELS.md) and [DeepSeek guide](https://github.com/Niko1221/Strata/blob/main/docs/DEEPSEEK41.md) have the measurements.

### First start

The first start downloads the model and prepares it. Later starts go straight to serving. The template's Overview explains each setting, and Strata's [Docker guide](https://github.com/Niko1221/Strata/blob/main/docs/DOCKER.md) covers them in full.

An API key is always on. If you leave it empty, the first start makes one; show it with:

```bash
docker exec Strata strata launch --show-key --preset docker
```

## Updates

Strata is in rapid development and changes often, so expect this template to change along with it: settings get added, renamed or removed as new releases come out. Installed apps keep their saved copy of the template, so a new setting only appears after you reinstall or add it by hand.

The template follows `latest-cuda13`, which Strata's maintainer moves to each new release when its image is published. Unraid shows the update on the Docker tab. To stay on one release, change the tag to a version such as `0.2.3-cuda13`.

## Credits and licence

[Strata](https://github.com/Niko1221/Strata) and its image are by Niko1221 and the Strata contributors, under the MIT licence. The icon is Strata's own. This repository is MIT licensed; see [LICENSE](LICENSE).
