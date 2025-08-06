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
  <img width="790" height="150" alt="Package_Name" src="https://github.com/user-attachments/assets/644be916-a496-4651-978c-d02e7a9fd91e" />
</p>

## Description
The {Package_Name}  is a configurable image preprocessing module designed to apply various types of blurring effects to images. 
## Build with
- OpenCV
- Wsl
- Redis
<br><br>


## Features
- ✅ Supports multiple blur types:
    - Crop Images
    - Blur Images 
- 🎛️ Configurable **image_resolution** for tuning the image as requested
- 📦 Easily integrates with existing Novavision-ai packages 
<br><br>


## General View of the Blur Package Structure
| Executor            | Input                      | Configs                         | Outputs        |
|---------------------|----------------------------|----------------------------------|----------------|
| `Executor A`      | InputImage                 | `Resolution`, `CropRate`     | OutputImage |
| `Executor B`  | InputImage, InputDetections| `Resolution`,`BlurType`     | OutputImage |


## Executors
There are 2 executors in Blur. 

- **Executor A:** Crop the given image by using requested resolution
- **Executor B:** Blurs the given image by using requested blur type


## Inputs
- Executor A takes the given image(s) from the source via InputImage
- Executor B takes the given image(s) from the source via InputImage and takes the detection list for an object from previous detection package

## Configurations
| Parameter     | Type | Description                                           | Default      |
| ------------- | ---- | ----------------------------------------------------- | ------------ |
| `Resolution` | str  | Size of the image                | `1920x1080`          |
| `BlurType`   | str  | Type of blur: `"Gaussian"`, `"Average"`, `"Median"`, `"Bilateral"` | `"Gaussian"` |
| `CropRate` | int  | Rate of the crop in a range (1-100)                 | `25`          |

There are one common configs for both Executor A and Executor B. 
1.  **BlurType:** There are 4 options to blur such as Gaussian, Average, Median and Bilateral. Functions use OpenCV fucntions to blur the given input.
2.  **Resolution**: It is a parameter that is used when creating the output in the requested sizes. High sizes of the images may cause problems about storage spaces.
      - 720p images are suggested for quality-storage space trade off.
3.  **CropRate**: It indicated ratio about cropped part over the whole image


## Outputs
- Executor A gives the blurred image(s) via OutputImage
- Executor B gives the image(s) with blurred objects via OutputImage


## Usage
1   . Executor A: It takes 1 input as InputImage and gives 1 output as OutputImage. Use of the Executor A can be seen at the image below.
![if_input](https://github.com/user-attachments/assets/b74abf1b-9c14-4a98-b876-5bd4f651e59c)

2   . Executor B: It takes 2 input as InputImage and InputDetections and gives 1 output as OutputImage. Use of the Executor B can be seen at the image below.
![detection_workflow](https://github.com/user-attachments/assets/3a977105-203a-43a0-bdf8-0d123d5014b9)
<br><br>


## Use Cases
### ✂️ 1. Executor A (Image Cropping)

> **Use Case:** Focus on relevant regions of interest before further processing.

**Scenario:**  
You are working with high-resolution surveillance footage, but only specific regions (e.g., detected faces or vehicles) are relevant. Instead of processing the entire image, you want to crop the important areas to reduce computation and improve clarity for downstream tasks.

- **Executor:** `Executor A`  
- **Input:** `InputImage`
- **Configs:** `CropRate = 25` (optional)  
- **Output:** Cropped image regions based on detection results.

### 🧊 2. Executor B (Image Blurring)
> **Use Case:** Apply anonymization or visual effects on entire or specific parts of an image.
**Scenario:**  
You are preparing a dataset for machine learning while protecting user privacy. Faces, license plates, or sensitive content must be blurred before publication or training.

- **Executor:** `Executor B`  
- **Inputs:** `InputImage`, `InputDetections`  
- **Configs:** `BlurType = "Gaussian"`, `Resolution = 1920x1080`  
- **Output:** Either the full image or detected regions are blurred depending on the input.


## Examples
1   .  Executor A with **BlurType:** Average & **KernelSize:** 51 
<br><br>
![if_average_51](https://github.com/user-attachments/assets/36c88595-878e-4304-89a4-bb7f16c248ec) 
<br><br><br><br>

2  .  Executor B with **BlurType:** Gaussian & **KernelSize:** 51
<br><br>
![df_gaussian_51](https://github.com/user-attachments/assets/ef140c6a-59f8-4b79-84fe-8633cc460037)
<br><br>


## Important Notes
- All the Blur functions including **Bilateral** filter are added into this package considering this [Roboflow Article: Image Blur Workflow](https://inference.roboflow.com/workflows/blocks/image_blur/). However Bilateral Filter does not work well for blurring compared to other methods. Despite this, it was kept in the package.

- [Project Report](https://docs.google.com/document/d/1AJtppCyoBo6kS0we25d6pRTlV03jA_eG8KQsy6pyfK0/edit?usp=sharing)

- [Project Test Documentation](https://docs.google.com/spreadsheets/d/1rCsDJH-zqn3cRMuv51Z539SmzpgLFfyM/edit?gid=1294141459#gid=1294141459)

- [Creating a good readme file](https://www.youtube.com/watch?v=a8CwpGARAsQ)
  
