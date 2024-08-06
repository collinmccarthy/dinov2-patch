# DINOv2 Integration

- Repo: [facebookresearch/dinov2](https://github.com/facebookresearch/dinov2/tree/main)
- Paper: [DINOv2: Learning Robust Visual Features without Supervision](https://arxiv.org/abs/2304.07193)
- Related paper: [Vision Transformers Need Registers](https://arxiv.org/abs/2309.16588)

## Add Submodule

```bash
cd ext_repos

# Original repo, many env-related issues
# git submodule add https://github.com/facebookresearch/dinov2.git dinov2_orig

# Newer PyTorch 2.4 compatible version
git submodule add https://github.com/zinccat/dinov2-patch.git dinov2
```

## Env with local CUDA

- Assuming CUDA 12.1 is installed locally, for building mmcv

```bash
cd ~/highres-av/ext_repos/env
conda env update -f dinov2_conda_req.yaml

# Install PyTorch, xFormers and other non-OpenMMLab pip packages
pip install -r dinov2_pip_req.txt

# Verify CUDA 12.1 is installed locally, since nvcc isn't available via conda
echo $CUDA_HOME

# Install OpenMMLab dependencies, building mmcv as necessary for PyTorch 2.x / CUDA 12.1
# See https://en.wikipedia.org/wiki/CUDA if you want to add/remove a CUDA architecture
# To output to a file for debugging, add `2>&1 | tee dinov2_mim_req_build_out.txt`
export FORCE_CUDA=1 && \
export MMCV_WITH_OPS=1 && \
export TORCH_CUDA_ARCH_LIST="6.0 6.1 6.2 7.0 7.2 7.5 8.0 8.6 8.7 8.9 9.0+PTX" && \
mim install -r dinov2_mim_req.txt --verbose --no-cache-dir

srun -p wario -c 12 mim install -r dinov2_mim_req.txt --verbose --no-cache-dir 2>&1 | tee dinov2_mim_req_build_out.txt
```

## Segmentation Inference

- See [dinov2.notebooks.semantic_segmentation.ipynb](./dinov2/notebooks/semantic_segmentation.ipynb)
- Need to add top-level `dinov2` proj dir to PYTHONPATH env variable