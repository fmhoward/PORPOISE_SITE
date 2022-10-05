### Retraining of Pan-Cancer Integrative Histology-Genomic Models using Site Preserved Cross Validation

[Original Cancer Cell Paper](https://www.cell.com/cancer-cell/fulltext/S1535-6108(22)00317-8) 

[Site Preserved Cross Validation](https://www.nature.com/articles/s41467-021-24698-1)


We present a repeat analysis of the recent paper by Chen et al from Cancer Cell using site-preserved cross validation, to determine if model performance was inflated due to batch effect.

Please follow installation guide as listed on [original PORPOISE repository](https://github.com/mahmoodlab/PORPOISE).

Training-validation splits from the original paper are found in the [splits/5foldcv](https://github.com/fmhoward/PORPOISE_SITE/tree/master/splits/5foldcv), and site-preserved splits are found in [splits/5foldcv_site](https://github.com/fmhoward/PORPOISE_SITE/tree/master/splits/5foldcv_site).

Features were extracted using the [CLAM package](https://github.com/mahmoodlab/CLAM) with the following commands:

'''
python create_patches_fp.py --source <SLIDE DIRCETORY> --save_dir <SAVE DIRECTORY> --patch_size 256 --seg --patch --stitch                                      
python extract_features_fp.py --data_h5_dir <H5 FILE FOLDER> --data_slide_dir <SLIDE DIRECTORY> --csv_path <CSV FROM ABOVE COMMAND> --feat_dir <FEATURE SAVE DIRCETORY> --batch_size 512 --slide_ext .svs
'''

To evaluate these splits with the SSN, MIL, and MMF networks, we used the following commands respectively:
'''
python main.py --which_splits <5foldcv or 5foldcv_site> --split_dir <CANCER TYPE> --mode omic --reg_type omic --model_type snn --apply_mutsig
python main.py --which_splits <5foldcv or 5foldcv_site> --split_dir <CANCER TYPE> --mode path --model_type porpoise_amil --apply_mutsig
python main.py --which_splits <5foldcv or 5foldcv_site> --split_dir <CANCER TYPE> --mode pathomic --reg_type pathomic --model_type porpoise_mmf --data_root_dir --apply_mutsig --fusion bilinear
```
