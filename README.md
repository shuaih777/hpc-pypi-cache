# hpc-pypi-cache

PyPI wheel cache for HPC environment where Artifactory is broken and PyPI is blocked.

## How it works

1. GitHub Actions downloads `.whl` files from PyPI
2. Wheels are uploaded as GitHub Release assets
3. From HPC, download and install locally:

```bash
# Download all wheels from latest release
gh release download --repo shuaih777/hpc-pypi-cache --dir ~/wheels/

# Install from local wheels
source ~/envs/llm_rec/bin/activate
pip install --no-index --find-links ~/wheels/ \
  transformers accelerate sentence-transformers peft einops omegaconf gensim
```

## Trigger a new download

Go to Actions → "Download Python wheels for HPC" → Run workflow

Or from CLI:
```bash
gh workflow run download-wheels.yml --repo shuaih777/hpc-pypi-cache
```
