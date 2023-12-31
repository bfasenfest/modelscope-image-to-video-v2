# Modelscope text to video synthesis model transformed to be text + image to video
## Training and inference scripts for creating videos from a static image

This project contains the code for transforming the modelscope text to video synthesis model to a text + image to video model and the code for generating videos with the new model.

This is an alternative and improved version of this [repo](https://github.com/motexture/modelscope-image-to-video-v1).

## Getting Started

### Installation
```bash
git clone https://github.com/motexture/ms-image-to-video
cd ms-image-to-video
```

### Python Requirements

```bash
pip install deepspeed
pip install -r requirements.txt
pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

Deepspeed is required if you want to use the training script.

## Preparing the config file for training
Open the training.yaml file and modify the parameters according to your needs.  <br /> 
upgrade_model should be True if you want to transform an old text to video modelscope model to the new text + image to video model.  <br /> 

## Train
```python
deepspeed train.py --config training.yaml
```
---

## Running inference
The `inference.py` script can be used to render videos with trained checkpoints.

Using a custom 2d text to image diffusion model for image conditioning:
```
python inference.py \
  --model checkpoint-path \
  --prompt "an astronaut is walking on the moon" \
  --model-2d stabilityai/stable-diffusion-2-1 \
  --num-frames 16 \
  --width 512 \
  --height 512 \
  --times 1 \
  --sdp
```

Animating a static image:
```
python inference.py \
  --model checkpoint-path \
  --prompt "an astronaut is walking on the moon" \
  --init-image "image.png" \
  --num-frames 16 \
  --times 1 \
  --sdp
```

Creating infinite length videos by using the last frame as the new init image and by increasing the --times parameter:
```
python inference.py \
  --model checkpoint-path \
  --prompt "an astronaut is walking on the moon" \
  --model-2d stabilityai/stable-diffusion-2-1 \
  --num-frames 16 \
  --width 512 \
  --height 512 \
  --times 4 \
  --sdp
```

## Shoutouts

- [ExponentialML](https://github.com/ExponentialML/Text-To-Video-Finetuning/) for the original training and inference code
- [bfasenfest](https://github.com/bfasenfest) for his contribution to the training and testing phases
- [Showlab](https://github.com/showlab/Tune-A-Video) and [bryandlee](https://github.com/bryandlee/Tune-A-Video) for their Tune-A-Video contribution
