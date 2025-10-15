# 🧭 Patch2Pix (CVPR 2021) — Quick Reproduction Roadmap

Reproducing **Patch2Pix: Epipolar-Guided Pixel-Level Correspondences (CVPR 2021)**  
using **pretrained NCNet** and Kaggle for training.

---

## 🪜 1. Clone Repository

```bash
git clone https://github.com/GrumpyZhou/patch2pix.git
cd patch2pix
```

---

## ⚙️ 2. Setup Environment

**Recommended:** Python 3.7 + PyTorch ≥ 1.7.0 + CUDA 10.2  
(Already available on Kaggle)

Or use Conda locally:

```bash
conda env create -f environment.yml
conda activate patch2pix
```

---

## 💾 3. Download Pretrained Models

Download pretrained NCNet and Patch2Pix weights:

```bash
cd pretrained
bash download.sh
```

This downloads:
- `ncn_ivd_5ep.pth` → pretrained NCNet
- `patch2pix_pretrained.pth` → pretrained Patch2Pix (optional)

---

## 🧠 4. Get Datasets

### Training — MegaDepth

Preprocessed version (same as D2Net):

```bash
cd data_pairs
bash download.sh
```

Or download manually: [MegaDepth Dataset](https://www.cs.cornell.edu/projects/megadepth/)

### Validation — PhotoTourism (ImMatch)

```bash
cd data
bash prepare_immatch_val_data.sh
python -m data_pairs.precompute_immatch_val_ovs --data_root data/immatch_benchmark/val_dense
```

---

## 🏗️ 5. Recreate Patch2Pix and train

Run training using the pretrained NCNet backbone:

```bash
python -m train_patch2pix --gpu 0 \
  --epochs 25 --batch 4 \
  --pretrain 'pretrained/ncn_ivd_5ep.pth' \
  --data_root 'data' -o 'output/patch2pix'
```

**🧩 For smaller GPUs (like on Kaggle):**  
Use `--batch 1 --ptmax 250` to reduce memory usage.

---

## 👀 6. Visualize Matches

Open the example notebook:

```bash
jupyter notebook examples/visualize_matches.ipynb
```

Replace the demo images with your own pair to test refined pixel-level correspondences.

---

## 🧪 7. (Optional) Benchmark Evaluation

To reproduce full benchmark results (HPatches, Aachen, InLoc):

Use [Image Matching Toolbox](https://github.com/GrumpyZhou/image-matching-toolbox)

---

## ✅ Quick Summary

| Step | Task | Source/Command |
|------|------|----------------|
| 1 | Clone repo | `git clone` |
| 2 | Setup env | `conda env create -f environment.yml` |
| 3 | Pretrained models | `pretrained/download.sh` |
| 4 | Datasets | `data_pairs/download.sh` + `prepare_immatch_val_data.sh` |
| 5 | Train model | `python -m train_patch2pix` |
| 6 | Visualize matches | `examples/visualize_matches.ipynb` |
| 7 | Evaluate (optional) | [Image Matching Toolbox](https://github.com/GrumpyZhou/image-matching-toolbox) |

---

## 📚 Reference

Zhou, Q., Sattler, T., Leal-Taixé, L.  
**"Patch2Pix: Epipolar-Guided Pixel-Level Correspondences."** CVPR 2021.  
[Paper Link](https://arxiv.org/abs/2012.01909)
