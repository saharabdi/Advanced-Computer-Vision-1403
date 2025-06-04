# YOLOv12 Object Detection: Demonstrations and Fine-Tuning

## Overview

This repository provides a comprehensive guide and practical examples for utilizing the YOLOv12 object detection model. It covers setup, inference with various YOLOv12 versions (small, medium, large), performance benchmarking, advanced visualization techniques, and a detailed walkthrough for custom fine-tuning YOLOv12-large on a domain-specific dataset (car components).

---

## Features

* Installation guide for YOLOv12 and essential libraries (`ultralytics`, `supervision`, `roboflow`).
* Inference examples using pre-trained YOLOv12s, YOLOv12m, and YOLOv12l models.
* Clear explanation and measurement of **Run Time vs. Inference Time**.
* Comparative insights between YOLOv12-small and YOLOv12-large performance.
* Targeted object detection (e.g., 'person' class) with effective class filtering.
* Visualization of detection results using both default Ultralytics plotting and the `supervision` library for enhanced customization.
* Step-by-step guide for **fine-tuning YOLOv12-large** on a custom "Car Components Dataset."
* Practical data augmentation strategies using `albumentations` to improve model robustness.
* Detailed model evaluation on validation and test sets, including metrics like **mAP@0.5** and **mAP@0.5:0.95**.
* Demonstration of inference on novel images using the custom fine-tuned model.

---

## Setup

To get started with the examples and fine-tuning scripts, install the necessary Python libraries:

```bash
# Install Ultralytics (provides the YOLO API and training framework)
pip install ultralytics

# Install the specific YOLOv12 implementation and the supervision library for advanced annotations
pip install -q git+[https://github.com/sunsmarterjie/yolov12.git](https://github.com/sunsmarterjie/yolov12.git) supervision

# For downloading custom datasets from Roboflow (as shown in the fine-tuning example)
pip install roboflow

# For data augmentation (used in the fine-tuning pipeline)
pip install albumentations

# For an image downloading utility used in one of the inference examples
pip install icrawler