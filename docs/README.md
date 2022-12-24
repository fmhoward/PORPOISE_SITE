### Retraining of Pan-Cancer Integrative Histology-Genomic Models using Site Preserved Cross Validation

[Original Cancer Cell Paper](https://www.cell.com/cancer-cell/fulltext/S1535-6108(22)00317-8) 

[Site Preserved Cross Validation](https://www.nature.com/articles/s41467-021-24698-1)


We present a repeat analysis of the recent paper by Chen et al from Cancer Cell using site-preserved cross validation, to determine if model performance was inflated due to batch effect.

Please follow installation guide as listed on [original PORPOISE repository](https://github.com/mahmoodlab/PORPOISE).

Training-validation splits from the original paper are found in the [splits/5foldcv](https://github.com/fmhoward/PORPOISE_SITE/tree/master/splits/5foldcv), and site-preserved splits are found in [splits/5foldcv_site](https://github.com/fmhoward/PORPOISE_SITE/tree/master/splits/5foldcv_site).

Features were extracted using the [CLAM package](https://github.com/mahmoodlab/CLAM) with the following commands:

```
python create_patches_fp.py --source <SLIDE DIRCETORY> --save_dir <SAVE DIRECTORY> --patch_size 256 --seg --patch --stitch                                      
python extract_features_fp.py --data_h5_dir <H5 FILE FOLDER> --data_slide_dir <SLIDE DIRECTORY> --csv_path <CSV FROM ABOVE COMMAND> --feat_dir <FEATURE SAVE DIRCETORY> --batch_size 512 --slide_ext .svs
```

We did not have 84 slides that were included in the original analysis available for our repeat analysis, and these were excluded from the MIL / MMF model performance tests These slides are listed in [missing.csv](https://github.com/fmhoward/PORPOISE_SITE/blob/master/missing.csv).

To evaluate these splits with the SSN, MIL, and MMF networks, we used the following commands respectively:
```
python main.py --which_splits <5foldcv or 5foldcv_site> --split_dir <CANCER TYPE> --mode omic --reg_type omic --model_type snn --apply_mutsig
python main.py --which_splits <5foldcv or 5foldcv_site> --split_dir <CANCER TYPE> --mode path --model_type porpoise_amil --apply_mutsig
python main.py --which_splits <5foldcv or 5foldcv_site> --split_dir <CANCER TYPE> --mode pathomic --reg_type pathomic --model_type porpoise_mmf --data_root_dir --apply_mutsig --fusion bilinear
```

We found performance (C-Index) as follows using the above commands and the published cross-folds
|TUMOR  |Site |SSN  |MIL  |MMF  |
|-------|-----|-----|-----|-----|
|BRCA   |0.578|0.586|0.591|0.608|
|HNSC   |0.534|0.516|0.571|0.595|
|LUAD   |0.585|0.554|0.564|0.633|
|LUSC   |0.576|0.522|0.553|0.562|
|KIRC   |0.589|0.633|0.566|0.61 |
|COAD   |0.597|0.58 |0.564|0.575|
|PAAD   |0.602|0.612|0.566|0.633|
|BLCA   |0.595|0.595|0.56 |0.63 |
|Average|0.582|0.575|0.567|0.606|

Performance (C-Index) declined in most cases using preserved-site cross validation
|TUMOR  |SSN  |MIL  |MMF  |
|-------|-----|-----|-----|
|BRCA   |0.542|0.559|0.534|
|HNSC   |0.476|0.564|0.535|
|LUAD   |0.585|0.571|0.628|
|LUSC   |0.523|0.559|0.533|
|KIRC   |0.652|0.589|0.654|
|COAD   |0.535|0.514|0.482|
|PAAD   |0.524|0.552|0.624|
|BLCA   |0.596|0.547|0.618|
|Average|0.554|0.557|0.576|

Absolute Decline in Performance per model / tumor type, and percent decline relative to a baseline performance of 0.5 representing random chance. In other words, if a C-Index declined from 0.6 to 0.55, approximately half of the predictive value of the model is lost (although in actuality performance loss is greater due to non-linearity in C-Index). We represent this loss as a percentage as follow - (0.6 - 0.55)/(0.6 - 0.5) * 100 = 50%.

|TUMOR  |SSN         |MIL         |MMF         |
|-------|------------|------------|------------|
|BRCA   |0.044 (51)  |0.032 (35)  |0.073 (68)  |
|HNSC   |0.039 (250) |0.008 (11)  |0.06 (63)   |
|LUAD   |-0.031 (-56)|-0.007 (-11)|0.005 (3)   |
|LUSC   |0 (-2)      |-0.006 (-11)|0.029 (47)  |
|KIRC   |-0.019 (-14)|-0.023 (-35)|-0.044 (-40)|
|COAD   |0.045 (56)  |0.05 (78)   |0.092 (124) |
|PAAD   |0.088 (79)  |0.014 (22)  |0.01 (7)    |
|BLCA   |-0.001 (-1) |0.013 (22)  |0.012 (9)   |
|Average|0.024 (32)  |0.01 (15)   |0.032 (30)  |


## Attribution
If you use this approach in your work or find it helpful, please consider citing our letter in <a href='https://doi.org/10.1016/j.ccell.2022.10.025'>Cancer Cell</a>.
```
@article{HOWARD2022,
title = {Multimodal deep learning: An improvement in prognostication or a reflection of batch effect?},
journal = {Cancer Cell},
year = {2022},
issn = {1535-6108},
doi = {https://doi.org/10.1016/j.ccell.2022.10.025},
url = {https://www.sciencedirect.com/science/article/pii/S1535610822005220},
author = {Frederick M. Howard and Jakob Nikolas Kather and Alexander T. Pearson}
}
```
