# R Bayesian DevContainer Image

A reproducible R environment for Bayesian statistics with micromamba package management and radian interactive console.

## Features

- **R 4.3** with tidyverse and data wrangling tools
- **Bayesian packages**: rstan, brms, rstanarm, cmdstanr, nimble, and more
- **Micromamba** for fast, reliable package management
- **Radian** enhanced R console with syntax highlighting
- **JupyterLab** with R kernel support
- **CmdStan** pre-installed via conda

## Available Images

- `ghcr.io/kantundpeterpan/devcontainer-base-images/r-bayesian:latest`

## Usage

### In devcontainer.json

```json
{
  "image": "ghcr.io/kantundpeterpan/devcontainer-base-images/r-bayesian:latest"
}
```

### Interactive R Console

```bash
radian
```

### Run R Script

```bash
Rscript myscript.R
```

### Jupyter Lab

```bash
jupyter lab
```

## Included Packages

### Data Wrangling
- tidyverse, data.table, dplyr, tidyr, readr, lubridate, stringr, janitor, arrow

### Plotting
- ggplot2, patchwork, plotly, gganimate, ggraph, igraph, bayesplot

### Modeling
- caret, glmnet, broom, recipes

### Bayesian Statistics
- rstan, brms, rstanarm, cmdstanr, nimble, posterior, bayesplot, loo, bridgesampling, bayestestR, tidybayes, coda, mcmc, mcmcse, runjags, rjags

### Interactive
- shiny, irkernel, jupyterlab, radian

## Building Locally

```bash
cd r-bayesian
docker build -t r-bayesian:latest .
```

## License

MIT
