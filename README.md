# MetaRoom Benchmark for VNN-COMP 2023
This is the MetaRoom Benchmark for [VNN-COMP 2023](https://sites.google.com/view/vnn2023). This public repo follows the instructions [here](https://github.com/stanleybak/vnncomp2023/issues/2) and [here](https://vnncomp.christopher-brix.de/benchmark/details).

## Introduction
MetaRoom Benchmark is based on [MetaRoom dataset](https://sites.google.com/view/metaroom-dataset/home), which aims at the robustness certification and verification of deep learning based vision models against camera motion perturbation in the indoor robotics application. Specifically, this benchmark focuses on the classification over 20 labels against camera moves within tiny perturbation radii of  translation along z-axis and rotation along y-axis (e.g. 1e-5 m, 2.5e-4 deg). More details about the dataset can be found in [PDF](https://proceedings.mlr.press/v205/hu23b.html). Download `dataset.zip` from [here](https://drive.google.com/file/d/1uiuAymh1E4QYAfA_VSblK_iUCxpCB5Dk/view?usp=sharing) and unzip it under the root path to test ONNX and VNNLIB.

## Details about ONNX and VNNLIB
For the ONNX, the pretrained [neural networks](https://github.com/Verified-Intelligence/auto_LiRPA/blob/master/examples/vision/models/feedforward.py) of `cnn_4layer` and `cnn_6layer` are converted from Pytorch models to ONNX models with customized `torch.nn.module` for image projection operator. The input is the one-dimension relative camera motion around 0 (e.g. [-1e-5, 1e-5]) and the output is for classification over 20 labels. The pretrained models give correct prediction at camera motion movement origins. Run 

``python generate_properties.py XX``

with random seed `XX` to generate random 100 test samples among different models and different perturbation types (y-axis rotation or z-axis translation), shown in `vnnlib/` and `instances.csv`.

## Test and pass random trials 
The ONNX models can be tested by running 

``python test_onnx.py --onnx_file ./onnx/XXX``

with the camera at the movement origin. Besides, following [randgen](https://github.com/stanleybak/simple_adversarial_generator.git), ONNX and VNNLIB files are also passed random trials. Please note that since the projection operator is customized, it is required to register it again (but only once) in the session when running using `onnxruntime` and `onnxruntime_extensions`. The modified codes are under `randgen` and can be run as 

``python randgen.py ../onnx/XXX ../vnnlib/YYY output_file``

### Citation
If you fine the repo useful, feel free to cite:

H. Hu, Z. Liu, L. Li, J. Zhu and D. Zhao "[Robustness Certification of Visual Perception Models via Camera Motion Smoothing](https://proceedings.mlr.press/v205/hu23b.html)", CoRL 2022
```
@InProceedings{pmlr-v205-hu23b,
  title = 	 {Robustness Certification of Visual Perception Models via Camera Motion Smoothing},
  author =       {Hu, Hanjiang and Liu, Zuxin and Li, Linyi and Zhu, Jiacheng and Zhao, Ding},
  booktitle = 	 {Proceedings of The 6th Conference on Robot Learning},
  pages = 	 {1309--1320},
  year = 	 {2023},
  volume = 	 {205},
  series = 	 {Proceedings of Machine Learning Research},
  month = 	 {14--18 Dec},
  publisher =    {PMLR}
}
```

### Reference
> - [camera-motion-smoothing](https://github.com/HanjiangHu/camera-motion-smoothing)
> - [randgen](https://github.com/stanleybak/simple_adversarial_generator)
> - [vnncomp2022_benchmarks](https://github.com/ChristopherBrix/vnncomp2022_benchmarks)


