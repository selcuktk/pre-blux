

<p align="center">
  <img width="760" height="225" alt="novavision-ai" src="https://github.com/user-attachments/assets/2ab133c5-1eed-4cba-ae96-e7066b63d697" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/language-Python-blue?logo=python" />
  <img src="https://img.shields.io/badge/containerized-Docker-blue?logo=docker" />
  <img src="https://img.shields.io/badge/env-WSL-lightgrey?logo=windows" />
  <img src="https://img.shields.io/badge/service-Redis-red?logo=redis" />
  <img src="https://img.shields.io/badge/library-OpenCV-green?logo=opencv" />
</p>


<p align="center">
  <img src="https://github.com/user-attachments/assets/4c932c6f-748a-4549-992c-d44f1082c21e" />
</p>



The Blur Component is a configurable image preprocessing module designed to apply various types of blurring effects to images. 
## Build with
- OpenCV
- Wsl
- Redis
<br><br>

## Features

- ✅ Supports multiple blur types:
    - Gaussian Blur
    - Average Blur
    - Median Blur
    - Bilateral Filter
- 🎛️ Configurable **kernel size** for tuning the blur intensity
- 📦 Easily integrates with existing Novavision-ai packages 
<br><br>


## General View of the Blur Package Structure
| Executor            | Input                      | Configs                         | Outputs        |
|---------------------|----------------------------|----------------------------------|----------------|
| `ImageFocused`      | InputImage                 | `KernelSize`, `BlurType`     | OutputImage |
| `DetectionFocused`  | InputImage, InputDetections| `KernelSize`,`BlurType`     | OutputImage |



## Executors
There are 2 executors in Blur. 

- **ImageFocused:** Blurs the given image completely 
- **DetectionFocused:** Blurs the detected objects on the given image 



## Inputs
- ImageFocused takes the given image(s) from the source via InputImage
- DetectionFocused takes the given image(s) from the source via InputImage and takes the detection list for an object from previous detection package
## Configurations



| Parameter     | Type | Description                                           | Default      |
| ------------- | ---- | ----------------------------------------------------- | ------------ |
| `BlurType`   | str  | Type of blur: `"Gaussian"`, `"Average"`, `"Median"`, `"Bilateral"` | `"Gaussian"` |
| `KernelSize` | int  | Size of the kernel (must be odd) (<52)                 | `25`          |

There are two common configs for both ImageFocused and DetectionFocused. 
1.  **BlurType:** There are 4 options to blur such as Gaussian, Average, Median and Bilateral. Functions use OpenCV fucntions to blur the given input.
2.  **KernelSize**: It is a parameter that is used on blurring functions. Some of the functions do not work with even numbers for kernel size. To make system simple, kernel size was restricted to be odd number for all blurring methods.
- A validator and a logger are added to the system considering if the users enter an even kernel size. The system gives the following warning message in this situation.

<img width="765" height="45" alt="kernel_log" src="https://github.com/user-attachments/assets/21ebb7e2-3917-43c0-9043-9a87f07b61e6" />

## Outputs
- ImageFocused gives the blurred image(s) via OutputImage
- DetectionFocused gives the image(s) with blurred objects via OutputImage
## Usage



1   . ImageFocused: It takes 1 input as InputImage and gives 1 output as OutputImage. Use of the ImageFocused can be seen at the image below.

![if_input](https://github.com/user-attachments/assets/b74abf1b-9c14-4a98-b876-5bd4f651e59c)


2   . DetectionFocused: It takes 2 input as InputImage and InputDetections and gives 1 output as OutputImage. Use of the DetectionFocused can be seen at the image below.

![detection_workflow](https://github.com/user-attachments/assets/3a977105-203a-43a0-bdf8-0d123d5014b9)
<br><br>

## Use Cases

### 📸 1. ImageFocused

> **Use Case:** Full-image anonymization or stylistic effect in datasets.

**Scenario:**  
You are preparing a dataset of public street view images to publish online. To protect individual privacy, you want to apply a uniform blur across the entire image, removing identifiable details while preserving general structure.

- **Executor:** `ImageFocused`  
- **Input:** `InputImage`  
- **Configs:** `BlurType = "Average"`, `KernelSize = 51`  
- **Output:** Entire image is blurred using average blur method.


### 🕵️ 2. DetectionFocused

> **Use Case:** Selective blurring of sensitive objects such as faces, license plates, or logos.

**Scenario:**  
You’ve run an object detection model on surveillance footage to detect faces. Now, instead of blurring the whole frame, you want to blur only the detected face regions to meet privacy requirements while preserving the rest of the scene.

- **Executor:** `DetectionFocused`  
- **Inputs:** `InputImage`, `InputDetections`  
- **Configs:** `BlurType = "Gaussian"`, `KernelSize = 25`  
- **Output:** The faces (or any detected object) are selectively blurred, rest of the image remains untouched.
## Screenshots
1   .  ImageFocused with **BlurType:** Average & **KernelSize:** 51 
<br><br>
![if_average_51](https://github.com/user-attachments/assets/36c88595-878e-4304-89a4-bb7f16c248ec) 
<br><br><br><br>

2.  DetectionFocused with **BlurType:** Gaussian & **KernelSize:** 51
<br><br>
![df_gaussian_51](https://github.com/user-attachments/assets/ef140c6a-59f8-4b79-84fe-8633cc460037)
<br><br>

## Important Notes

- All the Blur functions including **Bilateral** filter are added into this package considering this [Roboflow Article: Image Blur Workflow](https://inference.roboflow.com/workflows/blocks/image_blur/). However Bilateral Filter does not work well for blurring compared to other methods. Despite this, it was kept in the package.

- [Project Report](https://docs.google.com/document/d/1AJtppCyoBo6kS0we25d6pRTlV03jA_eG8KQsy6pyfK0/edit?usp=sharing)

- [Project Test Documentation](https://docs.google.com/spreadsheets/d/1rCsDJH-zqn3cRMuv51Z539SmzpgLFfyM/edit?gid=1294141459#gid=1294141459)
