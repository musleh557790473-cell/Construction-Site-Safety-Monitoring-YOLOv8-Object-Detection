# Class Definitions & Labeling Rules

## Overview

This document defines the target classes for the YOLOv8 construction safety detection model, including visual descriptions, labeling conventions, and edge-case guidance.

## Class Table

| ID | Class | Description | Annotation Rule |
|----|-------|-------------|-----------------|
| 0 | `worker` | Any person visible on the construction site | Full-body bounding box from head to feet |
| 1 | `helmet` | Hard hat / safety helmet being worn | Tight box around the helmet on the worker's head |
| 2 | `no-helmet` | Worker's head without a helmet | Box around the exposed head area |
| 3 | `vest` | High-visibility vest being worn | Box around the worker's torso showing the vest |
| 4 | `no-vest` | Worker's torso without a safety vest | Box around the torso area lacking a vest |
| 5 | `machinery` | Heavy construction equipment (cranes, excavators, bulldozers) | Box around the entire machine |
| 6 | `vehicle` | Site vehicles (trucks, forklifts, cement mixers) | Box around the full vehicle |
| 7 | `safety-barrier` | Barricades, traffic cones, safety fencing | Box around each barrier element |

> **Important:** Update class IDs and names to match your actual Roboflow dataset export `data.yaml`.

## Labeling Guidelines

### General Rules
- **Minimum box size:** 20 × 20 pixels — smaller objects are not labeled
- **Occlusion threshold:** Do not label objects that are >50% occluded
- **Truncation:** Label objects partially cut off by image borders if >50% is visible
- **One label per object:** Each physical entity gets one bounding box per class

### PPE-Specific Rules
- A single worker may have **multiple labels**: one `worker` box (full body), plus one `helmet` or `no-helmet` box, plus one `vest` or `no-vest` box
- If PPE status is ambiguous (e.g., worker too far away), do **not** label the PPE class — only label `worker`
- Side/rear views where vest visibility is unclear: label as `no-vest` only if clearly not wearing one

### Machinery & Vehicles
- Parked/stationary equipment is still labeled
- If a vehicle is partially behind a building (>50% visible), label it
- Distinguish `machinery` (construction-specific) from `vehicle` (transport)

### Safety Barriers
- Label each individual cone, barricade, or fence segment separately
- Connected fencing: one box per visible section (not the entire fence line)

## Edge Cases

| Scenario | Decision |
|----------|----------|
| Worker wearing a hood over a helmet | Label as `helmet` |
| Worker holding a helmet (not wearing it) | Label as `no-helmet` |
| Partially visible person (legs only) | Label as `worker` if >50% visible |
| Toy/miniature construction equipment | Do not label |
| Workers in background, very small (<20px) | Do not label |
| Reflective tape but no vest | Label as `no-vest` |
