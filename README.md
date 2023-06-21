# EquiformerV2: Improved Equivariant Transformer for Scaling to Higher-Degree Representations

This repository contains the official PyTorch implementation of the work "EquiformerV2: Improved Equivariant Transformer for Scaling to Higher-Degree Representations".
We provide the code for training the base model setting on the OC20 S2EF-2M and S2EF-All+MD datasets.


<p align="center">
	<img src="fig/equiformer_v2.png" alt="photo not available" width="98%" height="98%">
</p>

<p align="center">
	<img src="fig/equiformer_v2_oc20_results.png" alt="photo not available" width="98%" height="98%">
</p>

<p align="center">
	<img src="fig/equiformer_v2_adsorbml_results.png" alt="photo not available" width="98%" height="98%">
</p>


## Content ##
0. [Environment Setup](#environment-setup)
0. [Training](#training)
0. [File Structure](#file-structure)
0. [Checkpoints](#checkpoints)
0. [Citation](#citation)
0. [Acknowledgement](#acknowledgement)



## Environment Setup ##


### Environment 

See [here](docs/env_setup.md) for setting up the environment.


### OC20

The OC20 S2EF dataset can be downloaded by following instructions in their [GitHub repository](https://github.com/Open-Catalyst-Project/ocp/blob/5a7738f9aa80b1a9a7e0ca15e33938b4d2557edd/DATASET.md#download-and-preprocess-the-dataset).

For example, we can download the OC20 S2EF-2M dataset by running:
```
    cd ocp
    python scripts/download_data.py --task s2ef --split "2M" --num-workers 8 --ref-energy
```
We also need to download the `"val_id"` data split to run training.

After downloading, place the datasets under `datasets/oc20/` by using `ln -s`:
```
    cd datasets
    mkdir oc20
    cd oc20
    ln -s ~/ocp/data/s2ef s2ef
```

To train on different splits like All and All+MD, we can follow the same link above to download the datasets.


## Training ##


### OC20

1. We train EquiformerV2 on the OC20 **S2EF-2M** dataset by running:
    
    ```bash
        sh scripts/train/oc20/s2ef/equiformer_v2s/equiformer_v2s_N@12_L@6_M@2_splits@2M_g@multi-nodes.sh
    ```
    The above script uses 2 nodes with 8 GPUs on each node.
    
    If there is an import error, it is possible that [`ocp/ocpmodels/common/utils.py`](https://github.com/Open-Catalyst-Project/ocp/blob/5a7738f9aa80b1a9a7e0ca15e33938b4d2557edd/ocpmodels/common/utils.py#L329) is not modified. 
    Please follow [here](docs/env_setup.md) for details.

    We can also run training on 8 GPUs on 1 node:
    ```bash
        sh scripts/train/oc20/s2ef/equiformer_v2s/equiformer_v2s_N@12_L@6_M@2_splits@2M_g@8.sh
    ```

2. We train **EquiformerV2 (153M)** on OC20 **S2EF-All+MD** by running:
    ```bash
        sh scripts/train/oc20/s2ef/equiformer_v2s/equiformer_v2s_N@20_L@6_M@3_splits@all+md_g@multi-nodes.sh
    ```
    The above script uses 16 nodes with 8 GPUs on each node.

3. We train **EquiformerV2 (31M)** on OC20 **S2EF-All+MD** by running:
    ```bash
        sh scripts/train/oc20/s2ef/equiformer_v2s/equiformer_v2s_N@8_L@4_M@2_splits@all+md_g@multi-nodes.sh
    ```
    The above script uses 8 nodes with 8 GPUs on each node.
    

## File Structure ##

1. [`nets`](nets) includes code of different network architectures for OC20.
2. [`scripts`](scripts) includes scripts for training models on OC20.
3. [`main_oc20.py`](main_oc20.py) is the code for training, evaluating and running relaxation.
4. [`oc20/trainer`](oc20/trainer) contains code for the force trainer as well as some utility functions.
5. [`oc20/configs`](oc20/configs) contains config files for S2EF.


## Checkpoints ##

We provide the checkpoints of EquiformerV2 trained on S2EF-2M dataset for 30 epochs, EquiformerV2 (31M) trained on S2EF-All+MD, and EquiformerV2 (153M) trained on S2EF-All+MD.
|Model	|Split	|Download	|val force MAE (meV / Å) |val energy MAE (meV) |
|---	|---	|---	|---	|---	| 
|EquiformerV2	|2M	|[checkpoint](https://www.dropbox.com/s/rtdo05g12wh66vb/equiformer_v2_N%4012_L%406_M%402_epochs%4030_splits%402M.pt?dl=0) \| [config](oc20/configs/s2ef/2M/equiformer_v2s/equiformer_v2s_N@12_L@6_M@2_epochs@30.yml)	|19.4 | 278 |
|EquiformerV2 (31M)|All+MD |[checkpoint](https://www.dropbox.com/s/4es5poqeksz68sz/equiformer_v2_N%408_L%404_M%402_epochs%403_splits%40all%2Bmd.pt?dl=0) \| [config](oc20/configs/s2ef/all_md/equiformer_v2s/equiformer_v2s_N@8_L@4_M@2_31M.yml) |16.3 | 232 |
|EquiformerV2 (153M) |All+MD | [checkpoint](https://www.dropbox.com/s/m7jov7s6bwb3r89/equiformer_v2_N%4020_L%406_M%403_epochs%401_splits%40all%2Bmd.pt?dl=0) \| [config](oc20/configs/s2ef/all_md/equiformer_v2s/equiformer_v2s_N@20_L@6_M@3_153M.yml) |15.0 | 227 |


## Citation ##

Please consider citing the works below if this repository is helpful:

- EquiformerV2:

- [eSCN](https://arxiv.org/abs/2302.03655):
    ```bibtex
    @inproceedings{
        escn,
        title={{Reducing SO(3) Convolutions to SO(2) for Efficient Equivariant GNNs}},
        author={Passaro, Saro and Zitnick, C Lawrence},
        booktitle={International Conference on Machine Learning (ICML)},
        year={2023}
    }
    ```

- [Equiformer](https://arxiv.org/abs/2206.11990):
    ```bibtex
    @inproceedings{
        equiformer,
        title={{Equiformer: Equivariant Graph Attention Transformer for 3D Atomistic Graphs}},
        author={Yi-Lun Liao and Tess Smidt},
        booktitle={International Conference on Learning Representations (ICLR)},
        year={2023},
        url={https://openreview.net/forum?id=KwmPfARgOTD}
    }
    ```

- [OC20 dataset](https://arxiv.org/abs/2010.09990):
    ```bibtex
    @article{
        oc20,
        author = {Chanussot*, Lowik and Das*, Abhishek and Goyal*, Siddharth and Lavril*, Thibaut and Shuaibi*, Muhammed and Riviere, Morgane and Tran, Kevin and Heras-Domingo, Javier and Ho, Caleb and Hu, Weihua and Palizhati, Aini and Sriram, Anuroop and Wood, Brandon and Yoon, Junwoong and Parikh, Devi and Zitnick, C. Lawrence and Ulissi, Zachary},
        title = {{Open Catalyst 2020 (OC20) Dataset and Community Challenges}},
        journal = {ACS Catalysis},
        year = {2021},
        doi = {10.1021/acscatal.0c04525},
    }
    ```

Please direct questions to Yi-Lun Liao (ylliao@mit.edu).


## Acknowledgement ##

Our implementation is based on [PyTorch](https://pytorch.org/), [PyG](https://pytorch-geometric.readthedocs.io/en/latest/index.html), [e3nn](https://github.com/e3nn/e3nn), [timm](https://github.com/huggingface/pytorch-image-models), [ocp](https://github.com/Open-Catalyst-Project/ocp), [Equiformer](https://github.com/atomicarchitects/equiformer).