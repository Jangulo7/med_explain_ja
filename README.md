# An End-to-End Framework for Automated Generation and Visual Grounding of Medical Image

This repository contains the official implementation of the computational framework presented in the CLEF 2025 paper. The system provides an end-to-end, multi-stage pipeline for the automated generation of explanations for medical imagery, followed by their definitive visual grounding. 

The architecture initiates with a generative stage, where a Llava Llama 8B model performs initial concept detection and captioning, which is subsequently refined into a coherent explanation by Llama 3.1. These textual concepts are then processed through a sophisticated NLP pipeline, including Named Entity Recognition (NER), to extract and validate salient medical terms.

In the second major phase, we leverage Large Language Models as reasoning engines, providing the OpenAI GPT-4.1/4V APIs with the generated explanations and the source image to deduce approximate spatial coordinates for each medical entity. 

The final and pivotal stage of our framework, detailed in the accompanying Python script, ingests these LLM-generated coordinates and subjects them to a rigorous visual verification and segmentation process. This stage employs the Segment Anything Model (SAM), enhanced by heatmap-based confidence scoring and advanced computer vision heuristics (arrow-following, keypoint detection), to produce high-fidelity segmentation masks. A YOLOv8 model runs in parallel to identify any structures missed by the text-based pipeline.

The final output is a set of explainable images with bounding boxes that visually ground the initial AI-generated explanation in verifiable evidence. 

This work demonstrates a complete cycle from abstract text generation to concrete, pixel-level semantic grounding, bridging the gap between the generative capabilities of LLMs and the clinical need for transparent and reproducible diagnostic aids

UPDATE: A second version of the notebook is available with changes in YOLO enhancement. The system second stage employs the Segment Anything Model (SAM), enhanced by heatmap-based confidence scoring and advanced computer vision heuristics. Crucially, this stage also incorporates a multi-source YOLO detection strategy: a local YOLOv8 model provides initial broad object detection, which can be augmented by calls to a specialized, cloud-hosted medical YOLO model via the Roboflow API. Detections from all YOLO sources are strictly filtered for clinical relevance before being used to identify structures potentially missed by the text-driven pipeline.
