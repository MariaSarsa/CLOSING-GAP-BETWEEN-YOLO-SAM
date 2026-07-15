# YOLO vs. SAM with CLIP: Object Localization & Classification

This repository explores and compares two distinct computer vision pipelines for locating and classifying objects within a scene. It evaluates how **CLIP (Contrastive Language-Image Pre-training)** performs when paired with (**YOLO**) versus the segmantation model (**SAM - Segment Anything Model**).

<table>
  <tr>
    <td align="center">
      <img alt="Initial pipelines" src="https://github.com/user-attachments/assets/fe386d8b-95b4-4b68-9801-eb52dd5ceae6" width="50%"/>
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

## Repository Structure

The workspace is laid out as follows:

```text
├── Images/                
│   └── Object_Setup.jpeg  # Target test image
├── Prompt_SAM_CLIP.ipynb  # Pipeline: SAM segmentation + CLIP + CLIP matching
├── Prompt_YOLO_CLIP.ipynb # Pipeline: YOLO bounding boxes + CLIP matching
├── requirements.txt       # Python environment dependencies
└── README.md              # Project documentation
```


---

## Key Insight

While building this pipeline, an important trade-off was observed regarding how deep neural networks like CLIP interpret visual boundaries:

SAM’S mask is sharper and with a higher resolution, but for a deep neural network like CLIP, it is better to have the natural background context and padding provided by YOLO's bounding boxes. This maximizes classification accuracy.

<table>
  <tr>
    <td align="center">
      <img alt="Different masks" src="https://github.com/user-attachments/assets/876b131b-4dd9-472a-b82b-eeb39ca0b0c1" width="50%"/>
    </td>
  </tr>
  <tr>
    <td align="center">
      <sub>Side-by-side comparison of the same object: YOLO's bounding box retains surrounding context and padding, while SAM's mask is a higher resolution contour This difference in framing is the root cause of the accuracy gap seen with CLIP.<br>
      </sub>
    </td>
  </tr>
</table>

Another important consideration to make is that a pre-trained model like YOLO can only detect what it knows: it is limited to the fixed set of categories it was trained on. This could be seen as an initial disadvantage, considering that SAM detects with no "limits", but for the context of object detection with lenguage models, YOLO starts with a pre-categorization that pushes the language model.

So these two insights make YOLO + CLIP a better option than SAM + CLIP. But, can we find a way to align the performance and posibilities of SAM to the ones of YOLO?

This is what I thought myself, and the most basic idea was to just simply run CLIP twice, one with text-prompts with the same YOLO categories, so that we mimic what YOLO does. And an extra one that would be the standard run of CLIP.

This way, the pipeles for both YOLO and SAM are the following:

<table>
  <tr>
    <td align="center">
      <img alt="Closing the gap" src="https://github.com/user-attachments/assets/97ec77e3-8dd1-498a-8063-bcc14a6e8a91" width="50%"/>
    </td>
  </tr>
  <tr>
    <td align="center">
    </td>
  </tr>
</table>


---

## Results


<table>
  <tr>
    <td align="center">
      <img alt="YOLO+CLIP vs SAM+CLIP^2 testing prompts" src="https://github.com/user-attachments/assets/1419617e-b495-4074-9c0b-cc5e97417e4f" width="50%"/>
    </td>
  </tr>
  <tr>
    <td align="center">
      <sub>Results after closing the gap: running CLIP twice on the SAM pipeline (SAM + CLIP²) using YOLO-style category prompts plus a standard free-text prompt. Confidence scores and predicted labels are shown for both pipelines on the same test object.<br>
      </sub>
    </td>
  </tr>
</table>

Across all tested prompts, **YOLO + CLIP** produced higher-confidence correct classifications than **SAM + CLIP²**, confirming the trade-off described above. Despite the **SAM + CLIP²** adjustment, YOLO's natural context and padding still gives it a slight overall advantage.






---

## Conclusions

YOLO is fast and accurate but limited, while SAM is slower and offers more flexibility at the cost of added ambiguity. There is potential for deeper object understanding with such a simple pipeline.
