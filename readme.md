# Handheld Currency Detection Device for the Visually Impaired

## Overview# Indian Currency Detection for Visually Impaired People

## Overview

This repository contains a fork of the original **Handheld Currency Detection Device for the Visually Impaired** project, developed by Achal Nanjundamurthy and team at Jyothy Institute of Technology during 2024–2025.

The original project uses deep learning to recognize Indian currency notes and announce their denominations aloud, helping visually impaired people identify money more independently.

**Original repository:** https://github.com/achalnm/Currency_Recognition_RPi

**Published research paper:** [Currency Detection for the Visually Impaired Using Deep Learning — IJRASET, Volume 13, Issue V, May 2025](https://www.ijraset.com/best-journal/currency-detection-for-the-visually-impaired-using-deep-learning)

## Original Project Features

- Indian currency denomination recognition using MobileNetV2.
- TensorFlow Lite inference on a Raspberry Pi.
- Spoken denomination announcements using offline text-to-speech.
- Confidence-threshold handling for uncertain predictions.
- Camera-based currency image capture.

## Technologies

- Python
- TensorFlow and Keras
- TensorFlow Lite
- MobileNetV2
- OpenCV
- Pillow
- pyttsx3
- Raspberry Pi

## Original Model Details

| Property | Details |
|---|---|
| Model | MobileNetV2 |
| Input image size | 224 × 224 pixels |
| Output classes | 10, 20, 50, 100, 200, 500 rupees and other objects |
| Deployment format | TensorFlow Lite |
| Original target device | Raspberry Pi |

These details describe the original project and have not been independently revalidated in this fork.

## Planned Enhancements

The intended direction for this repository is to explore an accessible online application with:

- Manual image upload for currency recognition.
- Live camera-based recognition.
- English voice announcements.
- Clear feedback when a note cannot be identified.

These enhancements are planned and should not be considered implemented until they have been developed and tested.

## Original Authors and Attribution

The original project was developed by:

- Achal Nanjundamurthy
- Aruna A Shenoy
- Bhushan M V
- Pujitha D R

Please refer to the original repository and published paper for the original authors' contributions, project methodology, and publication details.

## Project Status

This repository currently contains the original project's code and materials through a GitHub fork. Any new functionality will be documented as it is implemented and tested.

## Disclaimer

This project is intended for educational and accessibility purposes. AI predictions may be incorrect. Do not rely on the system as the sole method of verifying a banknote or determining its value in critical transactions.

## License

Please check the original repository for its licensing terms before redistributing or modifying its contents. Any additional license for this fork should be established consistently with the original license and applicable permissions.

This was our final year B.E. project at Jyothy Institute of Technology (2024-2025). We wanted to build something that could genuinely help people. The device identifies Indian currency notes and reads the denomination aloud, so visually impaired individuals can use it independently without a smartphone or internet connection.

This project was accepted and published in the IJRASET Journal, Volume 13, Issue V, May 2025. The paper title is "Currency Detection for the Visually Impaired Using Deep Learning". Both the journal certificate and the full published paper are included in this repository.

[Read the published paper on IJRASET](https://www.ijraset.com/best-journal/currency-detection-for-the-visually-impaired-using-deep-learning)


## How It Works

When the user presses the tactile button, the Raspberry Pi captures a frame from the webcam. The image is resized to 224x224 and normalized before being passed to a fine-tuned MobileNetV2 model running as a TFLite interpreter entirely on-device. The class with the highest softmax probability is selected, and the denomination is spoken aloud using pyttsx3.

A confidence threshold of 0.75 is applied (defined as `CONFIDENCE_THRESHOLD` in [raspi.py](raspi.py)). If the model falls below this, the device says "Unable to identify the currency note. Please try again." This was added after testing revealed the model sometimes guessed wrong on crumpled or partially visible notes.

## Model and Architecture

![Inference Pipeline](outputs/architecture.svg)

The base model is MobileNetV2 pretrained on ImageNet. The last 30 layers are unfrozen for fine-tuning while the rest are frozen. Training uses data augmentation including rotation, zoom, brightness variation, and small translations to improve robustness in real-world lighting and positioning conditions. After training, the model is exported to TFLite format for deployment on the Pi.

| Property | Details |
| --- | --- |
| Base Model | MobileNetV2 (pretrained on ImageNet) |
| Training Framework | TensorFlow 2.13 / Keras |
| Deployment Format | TensorFlow Lite |
| Input Size | 224x224 pixels |
| Classes | 7 (10, 20, 50, 100, 200, 500 Rupees, Others) |
| Confidence Threshold | 0.75 |

## Hardware

- Raspberry Pi 3B+ or any newer version
- USB Webcam or Pi Camera (minimum 480p)
- Portable Battery Pack (5V, 3A) for untethered use
- Speaker or Headphones for audio output
- Tactile Push Button wired to a GPIO pin

## Software

- Python 3.8 or above
- TensorFlow / Keras for training
- tflite-runtime for on-device inference
- OpenCV
- pyttsx3 for offline text-to-speech
- Pillow

## Dataset Structure

```text
dataset/
├── 10/
├── 20/
├── 50/
├── 100/
├── 200/
├── 500/
└── others/
```

Each folder contains images of that denomination.

## Training

Update the path constants at the top of [trainh5.py](trainh5.py) and run the script. It fine-tunes the model, saves the best checkpoint using ModelCheckpoint, and automatically converts the saved `.h5` file to TFLite format at the end. Training ran for around 40 to 50 epochs before early stopping kicked in on our dataset.

## Deployment on Raspberry Pi

1. Transfer `currency_model.tflite` to the Raspberry Pi.
2. Install dependencies listed in `requirements.txt`.
3. Update `MODEL_PATH` in [raspi.py](raspi.py) to point to the model file.
4. Run `raspi.py` and press the button to trigger a capture and get the audio result.

## Publication

This project was accepted and published in the IJRASET Journal, Volume 13, Issue V, May 2025. The paper title is “Currency Detection for the Visually Impaired Using Deep Learning”. Both the journal certificate and the full published paper are included in this repository.

## Team

| Name | Student ID | Email |
| --- | --- | --- |
| Achal Nanjundamurthy | 1JT21CS003 | [achalnm02@gmail.com](mailto:achalnm02@gmail.com) |
| Aruna A Shenoy | 1JT21CS019 | [aruna.shenoy31@gmail.com](mailto:aruna.shenoy31@gmail.com) |
| Bhushan M V | 1JT21CS030 | [bhushanmv2003@gmail.com](mailto:bhushanmv2003@gmail.com) |
| Pujitha D R | 1JT21CS128 | [pujitharamesh2712@gmail.com](mailto:pujitharamesh2712@gmail.com) |
