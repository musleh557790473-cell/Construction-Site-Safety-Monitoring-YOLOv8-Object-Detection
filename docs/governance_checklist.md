# Governance Checklist — Construction Safety Detection Model

## 1. Privacy & Consent

| Item | Status | Notes |
|------|--------|-------|
| Are faces identifiable in the dataset? | ⚠️ Partially | Some images contain recognizable faces at close range |
| Has consent been obtained from individuals? | ⚠️ Limited | Dataset stored locally in repository; original consent status unknown |
| Is face blurring applied? | ❌ Not yet | Recommended before any deployment: apply face anonymization as a preprocessing step |
| Are personal identifiers stored? | ✅ No | No names, IDs, or personal information are associated with images |
| GDPR / local privacy compliance? | ⚠️ Review needed | If deployed in jurisdictions with strict privacy laws, a DPIA is recommended |

**Recommendations:**
- Apply face blurring or pixelation as a pre-processing step before deployment
- Obtain explicit consent documentation if using site-specific CCTV footage
- Do not store raw images longer than needed for model training/validation

---

## 2. Data Minimization

| Principle | Implementation |
|-----------|---------------|
| Collect only necessary data | Only construction site images with safety-relevant content |
| Limit data retention | Training data stored only during active project development |
| Minimize personal data | Model detects PPE objects, not individual identities |
| Avoid over-collection | Dataset focused on target classes only; irrelevant scenes excluded |

**Current status:** The model detects objects (helmets, vests, barriers) rather than identifying individuals. No biometric data is processed.

---

## 3. Limitations Statement — When NOT to Use This Model

This model should **NOT** be used for:

1. **Sole decision-making for safety compliance enforcement** — The model can miss violations (false negatives) and flag non-violations (false positives). Human oversight is always required.

2. **Worker identification or surveillance** — The model is designed to detect PPE compliance, not to identify, track, or monitor specific individuals.

3. **Legal evidence of safety violations** — Model predictions are not legally admissible and should not replace formal safety inspection documentation.

4. **Environments significantly different from training data** — The model was trained on daytime, outdoor construction sites. Performance may degrade in:
   - Indoor construction environments
   - Nighttime or very low-light conditions
   - Non-construction sites (manufacturing, mining, etc.)
   - Extreme weather (heavy rain, snow, fog)

5. **Critical safety-of-life decisions** — Never use this model as the sole system preventing access to dangerous zones or triggering emergency shutdowns.

---

## 4. Risk Assessment — False Negatives vs. False Positives

### False Negatives (Missed Detections) — **Higher Risk**

| Risk | Impact | Mitigation |
|------|--------|------------|
| Missed PPE violation | Worker without helmet/vest goes undetected → potential injury | Set lower confidence thresholds for safety-critical classes; pair with human monitoring |
| Missed worker in hazard zone | Worker near machinery undetected → serious injury risk | Use redundant detection systems; do not rely solely on AI |
| Missed barrier gap | Missing or moved barrier not flagged → unauthorized site access | Regular manual barrier inspections alongside automated monitoring |

**Risk Level: HIGH** — False negatives can directly lead to injuries. The model must be supplemented with human oversight.

### False Positives (False Alarms) — **Lower but Meaningful Risk**

| Risk | Impact | Mitigation |
|------|--------|------------|
| Non-violation flagged as violation | Worker correctly wearing PPE gets flagged → alert fatigue | Tune confidence threshold; add human review step for alerts |
| Background objects misclassified | Scaffolding flagged as barrier → incorrect safety zone mapping | Improve training data diversity; filter by confidence score |
| Excessive alerts | Too many false alarms → operators ignore real alerts | Batch alerts; prioritize high-confidence detections |

**Risk Level: MEDIUM** — False positives cause operational inefficiency and alert fatigue.

### Recommended Confidence Thresholds

| Use Case | Confidence Threshold | Rationale |
|----------|---------------------|-----------|
| PPE compliance monitoring | 0.30–0.40 | Favor recall — better to flag and verify than miss a violation |
| Site occupancy counting | 0.50 | Balance precision and recall |
| Automated reporting | 0.60+ | Higher threshold for official records |

---

## 5. License & Data Rights

### Project License
- **Code:** MIT License — see [LICENSE](../LICENSE)
- **Permitted:** Commercial and non-commercial use, modification, distribution
- **Required:** Include copyright notice and license text

### Dataset Rights
- **Source:** [Roboflow — Construction Safety](https://universe.roboflow.com/fyp1-dzcqx/construction-safety-rw6pc/browse?queryText=&pageSize=50&startingIndex=0&browseQuery=true), stored locally in `images dataset/`
- **License:** Subject to original dataset license on Roboflow
- **Usage:** Permitted for research and educational purposes
- **Redistribution:** Check original Roboflow dataset page for specific redistribution terms

### Model Weights
- **Derived from:** Ultralytics YOLOv8 (AGPL-3.0 base model)
- **Fine-tuned weights:** Subject to AGPL-3.0 for commercial use; educational/research use permitted
- **Note:** If deploying commercially, review Ultralytics licensing terms

---

## 6. Ethical Considerations

- **Bias awareness:** Model performance may vary across different PPE types, colors, and worker demographics. Regular testing across diverse conditions is recommended.
- **Worker dignity:** The system monitors equipment compliance, not worker behavior or productivity.
- **Transparency:** Workers on monitored sites should be informed that AI-assisted safety monitoring is in use.
- **Accountability:** A designated human safety officer must remain responsible for all safety decisions. AI outputs are advisory only.

---

*Last updated: [DATE]*  
*Author: [YOUR NAME]*
