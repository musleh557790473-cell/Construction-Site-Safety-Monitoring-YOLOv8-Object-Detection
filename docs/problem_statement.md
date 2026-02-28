# Problem Statement — Construction Site Safety Monitoring

## Background

The construction industry consistently ranks among the most dangerous sectors worldwide. Construction workers face elevated risks of injuries from falls, struck-by incidents, caught-in/between hazards, and electrocution — collectively known as the "Fatal Four."

A critical factor in site safety is **Personal Protective Equipment (PPE) compliance**. Hard hats, high-visibility vests, safety glasses, and steel-toed boots significantly reduce injury severity. However, manual compliance monitoring is:
- **Inconsistent** — supervisors cannot monitor all areas simultaneously
- **Reactive** — violations are often identified after incidents occur
- **Labor-intensive** — dedicated safety officers are costly and limited in coverage

## Problem Definition

**How can computer vision automate the detection of safety-critical objects and PPE compliance on construction sites to enable real-time monitoring and proactive intervention?**

## Scope

This project focuses on detecting the following categories:
1. **Workers** — presence and count of personnel on site
2. **PPE Compliance** — helmet and vest usage (worn vs. not worn)
3. **Heavy Machinery** — excavators, cranes, and other equipment
4. **Vehicles** — trucks, forklifts, site transport
5. **Safety Barriers** — barricades, cones, and fencing

## Success Criteria

| Criterion | Target | Rationale |
|-----------|--------|-----------|
| mAP@50 | ≥ 0.60 | Acceptable detection across all classes |
| Precision (PPE classes) | ≥ 0.70 | Minimize false alarms for compliance |
| Recall (PPE classes) | ≥ 0.65 | Catch most violations |
| Inference speed | ≥ 15 FPS | Enable real-time monitoring |
| Reproducibility | Full Colab pipeline | Third-party can replicate results |

## Stakeholders

- **Site Managers** — receive alerts for PPE violations
- **Safety Officers** — review compliance reports and trends
- **Workers** — benefit from safer working conditions
- **Regulatory Bodies** — access automated compliance documentation

## Constraints

- Model must run on consumer-grade GPU or cloud instances (e.g., Colab T4)
- Privacy considerations: worker faces should not be stored or identified
- Inference on CCTV-quality images (varying resolution, angle, lighting)
- Dataset sourced from [Roboflow — Construction Safety](https://universe.roboflow.com/fyp1-dzcqx/construction-safety-rw6pc/browse?queryText=&pageSize=50&startingIndex=0&browseQuery=true)

## Expected Impact

An effective system could:
- Reduce PPE violation response time from hours to seconds
- Provide continuous 24/7 monitoring coverage
- Generate automated compliance reports for audits
- Identify high-risk zones based on detection patterns
