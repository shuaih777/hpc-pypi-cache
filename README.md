# hpc-pypi-cache

HPC 的 Artifactory 缓存坏了，用这个 repo 中转 Python wheels。

## 步骤 1：在有外网的电脑上下载 wheels

```bash
mkdir ~/hpc_wheels && cd ~/hpc_wheels

pip download \
  --python-version 3.12 \
  --only-binary=:all: \
  regex transformers accelerate \
  sentence-transformers peft einops \
  omegaconf gensim tokenizers safetensors
```

## 步骤 2：上传到 HPC

```bash
scp -r ~/hpc_wheels/ l131341@posit003.am.lilly.com:/home/l131341/wheels/
```

## 步骤 3：在 HPC 上安装

```bash
source ~/envs/llm_rec/bin/activate

pip install --no-index --find-links /home/l131341/wheels/ \
  transformers accelerate sentence-transformers peft einops omegaconf gensim
```

## 环境信息

- HPC Python: 3.12.4 (`module load python/3.12.4`)
- 虚拟环境: `/home/l131341/envs/llm_rec`
- GPU 节点: NVIDIA RTX PRO 6000 Blackwell (96GB), CUDA 13.0
- PyTorch: 已装好 `torch==2.7.1+cu128`
- 已安装的基础包: numpy, pandas, scipy, datasets, lightgbm, huggingface_hub 等

## 注意事项

- HPC 无法直接访问 pypi.org（被防火墙拦截）
- Artifactory (`lrl-pypi`) 上大量 wheel 文件 404
- GitHub 可以访问，所以可以通过 GitHub release 或 scp 中转
- `--only-binary=:all:` 确保只下载 wheel 不下载源码包
- 如果 `pip download` 报 resolution-too-deep，去掉 `--only-binary=:all:` 试试
