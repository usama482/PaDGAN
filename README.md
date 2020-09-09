# PaDGAN

Experiment code associated with the paper: [PaDGAN: A Generative Adversarial Network for Performance Augmented Diverse Designs](https://arxiv.org/pdf/2002.11304.pdf)

![Alt text](/architecture.svg)

## License
This code is licensed under the MIT license. Feel free to use all or portions for your research or related projects so long as you provide the following citation information:

Wei Chen and Faez Ahmed. "PaDGAN: A Generative Adversarial Network for Performance Augmented Diverse Designs", In ASME 2020 International Design Engineering Technical Conferences and Computers and Information in Engineering Conference, IDETC/CIE 2020. American Society of Mechanical Engineers (ASME).

    @inproceedings{chen2020padgan,
	  title={PaDGAN: A Generative Adversarial Network for Performance Augmented Diverse Designs},
	  author={Chen, Wei and Ahmed, Faez},
	  booktitle={ASME 2020 International Design Engineering Technical Conferences and Computers and Information in Engineering Conference, IDETC/CIE 2020},
	  year={2020},
	  month={August},
	  organization={American Society of Mechanical Engineers (ASME)},
	  address={St. Louis, USA}
        }

## Required packages

- tensorflow < 2.0.0
- sklearn
- numpy
- matplotlib
- seaborn

## Usage

### Synthetic examples

1. Go to example directory:

   ```bash
   cd synthetic
   ```

2. Run single experiment:

   ```bash
   python run_experiment.py
   ```

   positional arguments:
    
   ```
   mode	train or evaluate
   data	dataset name (specified in datasets.py; available datasets are Ring2D, Grid2D, Donut2D, ThinDonut2D, and Arc2D)
   func	function name (specified in functions.py; available functions are Linear, MixGrid, MixRing, MixRing4, and MixRing6)
   ```

   optional arguments:

   ```
   -h, --help            	show this help message and exit
   --inf			maximize quality but not diversity
   --naive			use naive loss for quality
   --lambda0		coefficient controlling the weight of quality in the DPP kernel
   --lambda1		coefficient controlling the weight of the performance augmented DPP loss in the PaDGAN loss
   --disc_lr		learning rate for the discriminator
   --gen_lr		learning rate for the generator
   --id			experiment ID
   --batch_size		batch size
   --train_steps		training steps
   --save_interval 	number of intervals for saving the trained model and plotting results
   ```

   The default values of the optional arguments will be read from the file `synthetic/config.ini`.

   The trained model and the result plots will be saved under the directory `synthetic/trained_gan/<data>_<func>_GAN_<lambda0>_<lambda1>/<id>`, where `<data>`, `<func>`, `<lambda0>`, `<lambda1>`, and `<id>` are specified in the arguments or in `synthetic/config.ini`.
   
   Datasets and functions are defined in `synthetic/functions.py` and `synthetic/datasets.py`, respectively.
   
   Specifically, here are the dataset and function names (`<data>` and `<func>`) for the three synthetic examples in the paper:
   
   | Example | Dataset name | Function name |
   |---------|--------------|---------------|
   | I       | Grid2D       | MixRing4      |
   | II      | Donut2D      | MixRing6      |
   | III     | ThinDonut2D  | MixRing6      |
   
   Note that we can set `lambda0` and `lambda1` to zeros to train a vanilla GAN.

### Airfoil example

1. Go to example directory:

   ```bash
   cd airfoil
   ```

2. Download the airfoil dataset [here](https://drive.google.com/file/d/1Lk8yKy4UEDguz7p32lqx-sO4iHlXaCAC/view?usp=sharing) and extract the NPY files into `airfoil/data/`.


3. Go to the surrogate model directory:

   ```bash
   cd surrogate
   ```

4. Train a surrogate model to predict airfoil performances:

   ```bash
   python train_surrogate.py train
   ```

   positional arguments:
    
   ```
   mode	train or evaluate
   ```

   optional arguments:

   ```
   -h, --help            	show this help message and exit
   --save_interval		interval for saving checkpoints
   ```

5. Go back to example directory:

   ```bash
   cd ..
   ```

6. Run the experiment:

   ```bash
   python run_experiment.py train
   ```

   positional arguments:
    
   ```
   mode	train or evaluate
   ```

   optional arguments:

   ```
   -h, --help            	show this help message and exit
   --naive			use naive loss for quality
   --lambda0		coefficient controlling the weight of quality in the DPP kernel
   --lambda1		coefficient controlling the weight of the performance augmented DPP loss in the PaDGAN loss
   --id			experiment ID
   ```
 
   The default values of the optional arguments will be read from the file `airfoil/config.ini`.
 
   The trained model and the result plots will be saved under the directory `airfoil/trained_gan/<lambda0>_<lambda1>/<id>`, where `<lambda0>`, `<lambda1>`, and `<id>` are specified in the arguments or in `airfoil/config.ini`. Note that we can set `lambda0` and `lambda1` to zeros to train a vanilla GAN.

Please check out an example of using PaDGAN to address multivariate performance enhancement [here](https://github.com/wchen459/MO-PaDGAN).

