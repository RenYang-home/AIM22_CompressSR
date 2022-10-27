# AIM 2022 Challenge on Super-Resolution of Compressed Image and Video

The homepage for the AIM 2022 Challenge on Super-Resolution of Compressed Image and Video.  

This challenged is organized by [Ren Yang](https://renyang-home.github.io/) (ETH Zurich) and [Prof. Dr. Radu Timofte](https://people.ee.ethz.ch/~timofter/) (ETH Zurich, Julius Maximilian University of Wurzburg) as a part of the [AIM 2022 Workshop](https://data.vision.ee.ethz.ch/cvl/aim22/) in conjunction with [ECCV 2022](https://eccv2022.ecva.net/).

If the benchmark and the LDV 3.0 dataset are useful for your research, please cite:
```
@inproceedings{yang2022aim,
  title={{AIM 2022} Challenge on Super-Resolution of Compressed Image and Video: Dataset, Methods and Results},
  author={Ren Yang and Radu Timofte and others}, 
  booktitle={European Conference on Computer Vision Workshops}, 
  year={2022}
}
```

If you have questions, find dead links or bugs, please feel free to contact:

Ren Yang @ ETH Zurich, Switzerland   

Email: ren.yang@vision.ee.ethz.ch

## Challenge

### Track 1

In Track 1, images are first x4 downsampled and then compressed JPEG at quality = 10. We use the [DIV2K](https://data.vision.ee.ethz.ch/cvl/DIV2K/) in this track. The training and validation sets are at https://data.vision.ee.ethz.ch/cvl/DIV2K/. We do not release the test set. The test server at https://codalab.lisn.upsaclay.fr/competitions/5076 is also open. The researcher can submit their results to the server to get the results and compare with the results in this challenge.

The downsampled and compressed data are generated with the following commands

```
from PIL import Image # we use the Pillow library (version 7.2.0)

img = Image.open(path_gt + str(i).zfill(4) + '.png')

w, h = img.size

assert w % 4 == 0
assert h % 4 == 0

img = img.resize((int(w/4), int(h/4)), resample=Image.BICUBIC)

img.save(path + str(i).zfill(4) + '.jpg', "JPEG", quality=10)
```

The quality is evaluated in the RGB domain by PSNR.

### Track 2

In Track 2, videos are first x4 downsampled and then compressed in the YUV domain by the Low-delay P mode HM 16.20 at QP 37. The training, validation and test datasets are released at [LDV 3.0 dataset](https://github.com/RenYang-home/LDV_dataset#ldv-30-365-videos).

The raw YUV videos in the [LDV 3.0 dataset](https://github.com/RenYang-home/LDV_dataset#ldv-30-365-videos) are losslessly compressed to mkv via ffmpeg x265 to reduce the file sizes.

1. To generate the downsampled and compressed data, first convert the raw mkv files to YUV domain. The ffmpeg x265 decoder is recommended, e.g., “ffmpeg -i 001.mkv -pix_fmt yuv420p 001.yuv”

2. Downsample the videos by x2 bicubic (Track 2) or x4 bicubic (Track 3) using the commands below. We define ```w``` and ```h``` as the width and height after downsampling. In Track 2, ```w = width/4, h = height/4```.

```
ffmpeg -pix_fmt yuv420p -s (width)x(height) -i xxx.yuv -vf scale=(w)x(h):flags=bicubic xxx.yuv
```

3. Download HM 16.20 at https://hevc.hhi.fraunhofer.de/svn/svn_HEVCSoftware/tags/HM-16.20 or https://data.vision.ee.ethz.ch/reyang/HM16.20.zip

4. Compress yuv files by the command:

```
path_to_HM16.20/bin/TAppEncoderStatic 
-c path_to_HM16.20/cfg/encoder_lowdelay_P_main.cfg 
-c path_to_HM16.20/cfg/per-sequence/BasketballDrill.cfg 
-i xxx.yuv -q 37 -wdt (w) -hgt (h)
-f (frame_num) -fr (frame_rate) -b xxx.mkv
```

Note that “BasketballDrill.cfg” is a randomly selected file, and most of its information are replaced by the following configurations. Width, height, frame number and frame rate values are available in the info excel files (refer to [LDV 3.0 Dataset](https://github.com/RenYang-home/LDV_dataset#ldv-30-365-videos)).

5. The quality is evaluated in the RGB domain by PSNR. Please convert raw, compressed (and enhanced) videos to RGB domain for evaluation, e.g.,
```
ffmpeg -i path_to_raw/001.mkv ./raw_001/%3d.png
ffmpeg -i path_to_compressed/001.mkv ./001/%3d.png
```

## Publications in the challenges

[CIDBNet: A Consecutively-Interactive Dual-Branch Network for JPEG Compressed Image Super-Resolution]()

Xiaoran Qin (Institute of Automation, Chinese Academy of Sciences); Yu Zhu (School of Computer Science and Engineering,Anhui University); Chenghua Li (Institute of Automation Chinese Academy of Sciences); Peisong Wang (Institute of Automation, Chinese Academy of Sciences); Jian Cheng ("Chinese Academy of Sciences, China")

[Hierarchical Swin Transformer for Compressed Image Super-Resolution](https://arxiv.org/pdf/2208.09885)

Bingchen Li (Unversity of Science and Technology of China); Xin Li (University of Science and Technology of China); yiting lu (University of Science and Technology of China); Sen Liu (University of Science and Technology of China); Ruoyu Feng (University of Science and Technology of China); Zhibo Chen (University of Science and Technology of China)

[Swin2SR: SwinV2 Transformer for Compressed Image Super-Resolution and Restoration](https://arxiv.org/pdf/2209.11345)

Marcos V. Conde (University of Würzburg); CHOI Ui-Jin (MegastudyEdu); Maxime Burchi (University of Würzburg); Radu Timofte (University of Wurzburg & ETH Zurich)
