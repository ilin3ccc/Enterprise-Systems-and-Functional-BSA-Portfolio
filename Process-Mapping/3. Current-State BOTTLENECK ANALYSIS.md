# Bottleneck Analysis (Current-State): ICU Early Warning System

## 1. Context & Scope
This analysis examines the current state ICU Early Warning System (EWS) workflow from the moment patient vitals are captured to the point of clinical intervention and coordination. The workflow involves three primary actors — **Nurse**, **Clinician**, and **ICU Coordinator** — supported by the EWS system. The scope includes data capture, alert generation, clinical validation, and coordination activities.

## 2. Diagnostic Methodology
Three diagnostic lenses were applied:

- **Manual vs Automated Processes** — to identify steps where human input introduces latency or variability.
- **Decision Points** — to evaluate where human judgment affects workflow timing.
- **System Boundaries** — to assess information flow across actors and systems.

These lenses were selected to isolate structural bottlenecks without expanding scope beyond the current state workflow.

## 3. Bottleneck Identification
Three bottlenecks were identified:

- **Bottleneck #1 — Manual Nurse Entry**  
  Latency and variability introduced by manual recording and transcription of vital signs.

- **Bottleneck #2 — Clinician Decision Point**  
  Workflow dependency on variable human judgment when validating alerts.

- **Bottleneck #3 — ICU Coordinator Visibility**  
  Delayed information flow to the coordinator, who only sees alerts after clinician intervention decisions.

## 4. Evidence

### Bottleneck #1 — Manual Nurse Entry
The BPMN shows that vital signs are manually recorded and entered by the nurse before reaching the EWS. This creates a single human‑dependent entry point where delays or inconsistencies propagate downstream. The manual vs automated lens highlights this step as structurally vulnerable to latency.

### Bottleneck #2 — Clinician Decision Point
The workflow cannot progress until the clinician validates the alert. The BPMN shows this decision as a human judgment step following alert generation. The decision points lens identifies this as a structural dependency that introduces timing variability. 

### Bottleneck #3 — ICU Coordinator Visibility
The coordinator receives information only after the clinician decides to intervene. The BPMN shows no upstream visibility for the coordinator. The system boundaries lens highlights this delayed information flow as a coordination bottleneck.

## 5. Impact Analysis

### Bottleneck #1 — Manual Nurse Entry
Because this step initiates the workflow, any delay or inconsistency affects all downstream activities. Alert timing and clinical decision making depend on the accuracy and timeliness of this data, making the entire workflow sensitive to manual variability.

### Bottleneck #2 — Clinician Decision Point
Variability in decision timing affects when intervention steps can begin. Downstream coordination is also delayed because the coordinator cannot act until the clinician completes the validation step. This creates unpredictability in workflow throughput.

### Bottleneck #3 — ICU Coordinator Visibility
Delayed visibility prevents the coordinator from preparing for intervention until after the clinician’s decision. This affects coordination timing and reduces predictability across downstream steps.

## 6. Recommendations

### Bottleneck #1 — Manual Nurse Entry
Introduce system‑supported ingestion of vital signs to reduce reliance on manual transcription. This addresses the manual vs automated bottleneck by improving data consistency and reducing latency. Shifting initial data capture toward automated processes stabilizes alert timing and downstream decision making.

### Bottleneck #2 — Clinician Decision Point
Enhance the alert payload with severity categorization and consolidated clinical context. This reduces the amount of manual information gathering required before decision making. Aligning with the decision points lens, improved information flow reduces timing variability and increases workflow predictability.

### Bottleneck #3 — ICU Coordinator Visibility
Provide the ICU coordinator with earlier visibility into system‑generated alerts. Parallel alert delivery reduces dependency on the clinician’s decision as the sole trigger for coordination. This aligns with the system boundaries lens by improving cross‑actor information flow and enabling earlier preparation.

## 7. Future State Linkage

### Bottleneck #1 → Future State Change
The future state workflow introduces automated vitals ingestion alongside manual entry, reflected as parallel system and nurse tasks. This removes the single manual dependency and stabilizes alert generation.

### Bottleneck #2 → Future State Change
The future state BPMN includes a system task that enriches alerts with severity and context before clinician review. This reduces decision latency and supports more predictable intervention timing.

### Bottleneck #3 → Future State Change
The future state swimlane shows alerts routed simultaneously to both clinician and coordinator. This structural change removes the dependency on clinician validation for coordinator visibility and improves coordination readiness.
