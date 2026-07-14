# YOLO vs. SAM with CLIP: Object Localization & Classification

This repository explores and compares two distinct computer vision pipelines for locating and classifying objects within a scene. It evaluates how **CLIP (Contrastive Language-Image Pre-training)** performs when paired with (**YOLO**) versus the segmantation model (**SAM - Segment Anything Model**).

<table>
  <tr>
    <td align="center">
      <img alt="Initial pipelines" src="https://github.com/user-attachments/assets/fe386d8b-95b4-4b68-9801-eb52dd5ceae6" width="80%"/>
    </td>
  </tr>
  <tr>
    <td align="center">
      <sub>Comparison of the intial two pipelines: an input image and text prompt are passed through either YOLO (bounding boxes) or SAM (segmentation masks), and the resulting crop is classified by CLIP to produce the selected object.<br>
      </sub>
    </td>
  </tr>
</table>

---

## Key Insight

While building this pipeline, an important trade-off was observed regarding how deep neural networks like CLIP interpret visual boundaries:

SAM’S mask is sharper and with a higher resolution, but for a deep neural network like CLIP, it is better to have the natural background context and padding provided by YOLO's bounding boxes. This maximizes classification accuracy."*

---

## Repository Structure

The workspace is laid out as follows:

```text
├── Images/                
│   └── Object_Setup.jpeg  # Target test image
├── Prompt_SAM_CLIP.ipynb  # Pipeline: SAM segmentation + CLIP + CLIP matching
├── Prompt_YOLO_CLIP.ipynb # Pipeline: YOLO bounding boxes + CLIP matching
├── requirements.txt       # Python environment dependencies
└── README.md              # Project documentation
