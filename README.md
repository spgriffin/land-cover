# Large Scale High-Resolution Land Cover Mapping with Multi-Resolution Data

Repo accompanying the CVPR 2019 paper "Large Scale High-Resolution Land Cover Mapping with Multi-Resolution Data".

<!--
## Results

## Pre-trained models

The following *High-resolution only* models are generated by running `run_all_hr_experiments.py`:
- [Trained on 'de_1m_2013']()
- [Trained on 'ny_1m_2013']()
- [Trained on 'md_1m_2013']()
- [Trained on 'pa_1m_2013']()
- [Trained on 'va_1m_2014']()
- [Trained on 'wv_1m_2014']()


The following *High-resolution + Super-resolution* models are generated by running `run_all_hr+sr_experiments.py`:
- [Trained on 'de_1m_2013']()
- [Trained on 'ny_1m_2013']()
- [Trained on 'md_1m_2013']()
- [Trained on 'pa_1m_2013']()
- [Trained on 'va_1m_2014']()
- [Trained on 'wv_1m_2014']()


We also have trained an additional `unet_large` model using _all_ the training patches (with *high-resolution* training), available [here]().
-->

## Data

The dataset and its documentation can be found at [LILA.science](http://lila.science/datasets/chesapeakelandcover). The `download_all.sh` script will download and unzip a copy of this dataset to the `./chesapeake_data/` directory. Note: this script assumes that the `azcopy` program is on your PATH; `azcopy` can be downloaded [here](https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy-v10).

## Requirements

Pre-trained keras models were generated with Python 3.6. See `requirements.txt` for the library versions that we used when developing. Generally, these libraries are needed:

- numpy
- pandas
- tensorflow-gpu
- Keras
- segmentation-models
- shapely
- rasterio

### Install azcopy
```
wget https://aka.ms/downloadazcopy-v10-linux
tar -xvf downloadazcopy-v10-linux
sudo cp ./azcopy_linux_amd64_*/azcopy /usr/bin/
```
Only works with `sudo azcopy`.

## Usage

Make use of `run.py` to run an end to end process (training, testing and evaluation):
```
nohup python run.py --name example_config
```

When leaving all arguments to default, they will be fetched from `landcover/config.py`.

If you'd like to overwrite the defaults, you can set the arguments in the command line:
```
nohup python run.py -v 3 --data-dir chesapeake_data/ --output-dir results/ \
--training-states ny_1m_2013 --validation-states ny_1m_2013 \
--test-states ny_1m_2013  --name example --model unet --batch-size 16
```
Or edit `landcover/config.py`.

**Note:** the nohup command ensures the process keeps running even if the terminal is disconnected. It will also save an output with the logs in a `nohup.txt` file.

## Test

Run all tests:
```
pytest --pylint --cov=landcover/
```

Format code:
```
black
```
## References

Please cite the following papers if you use this work:

```
@inproceedings{robinson2019large,
  title={Large Scale High-Resolution Land Cover Mapping With Multi-Resolution Data},
  author={Robinson, Caleb and Hou, Le and Malkin, Kolya and Soobitsky, Rachel and Czawlytko, Jacob and Dilkina, Bistra and Jojic, Nebojsa},
  booktitle={Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition (CVPR)},
  year={2019},
  url={http://openaccess.thecvf.com/content_CVPR_2019/html/Robinson_Large_Scale_High-Resolution_Land_Cover_Mapping_With_Multi-Resolution_Data_CVPR_2019_paper.html}
}

@inproceedings{malkin2018label,
  title={Label super-resolution networks},
  author={Malkin, Kolya and Robinson, Caleb and Hou, Le and Soobitsky, Rachel and Czawlytko, Jacob and Samaras, Dimitris and Saltz, Joel and Joppa, Lucas and Jojic, Nebojsa},
  booktitle={International Conference on Learning Representations (ICLR)},
  year={2019},
  url={https://openreview.net/forum?id=rkxwShA9Ym},
}
```

## Todo

- Change `train_model_landcover.py` to save models without the superres loss, jaccard loss, or Lambda layers as these can cause problems with saving/loading in different versions.
- Create a test script that computes accuracy on the fly without saving model results.
