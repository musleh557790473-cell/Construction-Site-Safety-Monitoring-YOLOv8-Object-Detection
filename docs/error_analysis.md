# Error Analysis — YOLOv8 Construction Safety Model

## Overview

This document analyzes the failure modes of the trained YOLOv8 model on validation and new images. Understanding these errors is critical for guiding data collection, re-labeling, and model improvement.

---

## False Positives (Model detected something that isn't there)

### FP-1: Background Structures Detected as Safety Barriers
- **What:** The model frequently labels structural elements like scaffolding poles, railings, and building edges as `safety-barrier`.
- **Why:** Safety barriers and structural elements share similar visual features — long, thin, linear shapes with similar colors (orange/yellow). The training data may not have enough negative examples of scaffolding.
- **Evidence:** See `results/evidence/val_predictions/fp1_scaffold_as_barrier.png`

### FP-2: Non-Construction Headwear Detected as Helmet
- **What:** Baseball caps, beanies, and even certain hairstyles are sometimes classified as `helmet`.
- **Why:** The model relies heavily on head-region features and round shapes. Hard hats and caps share a similar silhouette from certain angles, especially at low resolution.
- **Evidence:** See `results/evidence/val_predictions/fp2_cap_as_helmet.png`

### FP-3: Parked Personal Cars Detected as Machinery
- **What:** Large personal vehicles (SUVs, pickup trucks) near the construction area are occasionally classified as `machinery`.
- **Why:** Color similarity (yellow/orange trucks) and proximity to construction context bias the model. The dataset may lack diverse negative examples of non-construction vehicles.
- **Evidence:** See `results/evidence/val_predictions/fp3_car_as_machinery.png`

---

## False Negatives (Model missed something that is there)

### FN-1: Distant Workers Not Detected
- **What:** Workers more than ~30 meters from the camera (appearing <30px tall) are frequently missed by the model.
- **Why:** YOLOv8 Nano at 640×640 has limited resolution for small objects. Distant workers lack distinguishable features, and many training images are close-to-medium range.
- **Evidence:** See `results/evidence/val_predictions/fn1_distant_worker.png`

### FN-2: Safety Barriers in Cluttered Scenes
- **What:** Barriers placed among piles of materials, equipment, and debris are often missed.
- **Why:** High visual clutter causes the barrier to blend with the background. Orange cones next to orange machinery are especially problematic. The model may need more training examples of barriers in cluttered contexts.
- **Evidence:** See `results/evidence/val_predictions/fn2_barrier_in_clutter.png`

### FN-3: Workers in Unusual Poses (Bending, Climbing)
- **What:** Workers who are crouching, bending over, or climbing ladders are sometimes not detected as `worker`.
- **Why:** Most training images show workers standing upright. Non-standard poses change the aspect ratio and silhouette significantly. The model has not seen enough pose diversity during training.
- **Evidence:** See `results/evidence/val_predictions/fn3_unusual_pose.png`

---

## Prioritized Next Data Improvements

### 1. Add Negative Hard Examples for Safety Barriers
- **Action:** Collect 200+ images of scaffolding, railings, and structural elements that should NOT be labeled as barriers
- **Expected impact:** Reduce FP-1 by ~40–50%
- **Priority:** High — this is the most frequent false positive

### 2. Increase Small Object Representation
- **Action:** Add 150+ images with distant/small workers (zoomed-out site views, aerial angles) and annotate them carefully
- **Augmentation:** Use tiling or SAHI (Slicing Aided Hyper Inference) during inference
- **Expected impact:** Improve recall for distant workers by ~20–30%
- **Priority:** High — missed workers are a critical safety concern

### 3. Diversify Worker Pose Training Data
- **Action:** Collect 100+ images of workers in non-standard poses (bending, climbing, kneeling, carrying objects)
- **Augmentation:** Apply random rotation ±15° and scale augmentations
- **Expected impact:** Improve worker recall in complex scenarios by ~15–25%
- **Priority:** Medium — impacts specific but important scenarios

---

## Summary Table

| Error Type | ID | Class Affected | Root Cause | Severity |
|------------|-----|----------------|------------|----------|
| False Positive | FP-1 | safety-barrier | Visual similarity with scaffolding | Medium |
| False Positive | FP-2 | helmet | Cap/hat shape similarity | Low |
| False Positive | FP-3 | machinery | Vehicle color/context bias | Low |
| False Negative | FN-1 | worker | Small object resolution limit | High |
| False Negative | FN-2 | safety-barrier | Cluttered scene background | Medium |
| False Negative | FN-3 | worker | Pose variation gap in training | Medium |
