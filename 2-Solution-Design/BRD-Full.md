# Business Requirements Document (BRD)

## ICU Early Warning System (EWS)

## 1. Executive Summary & Strategic Context

The ICU is a high-stakes, time-sensitive environment where timely information, rapid decision-making, and team coordination are paramount to reducing clinical risk. Early detection of deterioration signals and timely escalation enable care teams to coordinate interventions effectively. To support this, the Early Warning System must reduce human-driven delays, eliminate blind spots, and ensure reliable escalation across all roles to strengthen overall clinical responsiveness.

## 2. Problem Statement & Business Value

### 2.1 Core Operational Problems

Our current ICU EWS contains several points where information flow is delayed or inconsistent. Manual vitals recording inherently creates delays and timing variability that affect downstream alert generation. These undifferentiated alerts delay triage, rely heavily on clinician validation, and prevent the ICU coordinator from preparing early.

### 2.2 Impact on Workflow & Roles

These issues disrupt the timing and clarity of information passed between nurses, clinicians, and the ICU coordinator. Clinicians delay triage because alerts lack differentiation, making it harder to quickly assess severity. In addition, the ICU coordinator receives alerts too late to prepare proactively, reducing overall intervention readiness.

### 2.3 Business Value of Fixing These Problems

By improving information flow and alert clarity, we can strengthen early detection and speed up triage. With lowered variability, we also reduce clinical risk by enabling faster and more decisive intervention decisions. Structured escalation paths guide care teams to coordinate more predictably and reduce unnecessary delays. Altogether, these improvements support the ICU’s goal of delivering timely, reliable, and proactive patient care.

## 3. Scope Boundaries

### 3.1 In Scope

- **ICU EWS Workflow** — The end-to-end deterioration detection process, including vital sign capture, scoring, alert generation, clinician review, ICU coordination, and documentation.

- **Vital Sign Capture Process** — The two supported methods of vital sign entry (manual nurse entry and automated device ingestion) that feed the EWS scoring engine.

- **EWS Scoring Logic** — The data-backed thresholds, scoring rules, and algorithmic triggers that determine when an alert is generated.

- **Alert Generation & Routing** — The enriched alerting mechanism that prioritizes alerts, provides contextual information, and routes notifications to clinicians and ICU coordinators.

- **Clinician Review Workflow** — The clinician’s validation, response, and documentation steps following alert receipt.

- **ICU Coordination Workflow** — The ICU coordinator’s parallel alert receipt, escalation readiness tasks, and dashboard updates based on clinician intervention decisions.

### 3.2 Out of Scope

- **Non-ICU Clinical Workflows** — Any workflows outside the ICU (e.g., Emergency Department, General Ward, Surgical Units) are excluded from this redesign.

- **EMR System Enhancements** — Changes to EMR data structures, data governance, UI components, or underlying EMR platform functionality are not included.

- **Staffing or HR Policy Changes** — Staffing levels, hiring decisions, scheduling adjustments, or role reassignments are not part of this project.

- **Medical Device Replacement** — Replacement or upgrade of bedside monitors, vital-sign devices, or any clinical hardware is out of scope.

- **Hospital-Wide Alert Systems** — Enterprise alerting systems outside the ICU Early Warning System will not be redesigned or modified.

### 3.3 Assumptions

- **Technical Stability** — The existing EMR and EWS platforms will remain stable, available, and operational throughout the redesign.

- **Consistent Vital Sign Capture** — Nurses will continue capturing vital signs consistently and accurately, whether manually or via automated devices.

- **Clinician & Coordinator Availability** — Clinicians and ICU coordinators will remain available and accountable for responding to alerts within accepted timeframes.

- **Data Quality & Integrity** — All vital-sign data entering the EWS will be complete, accurate, and properly timestamped.

- **Alert Threshold Governance** — Clinical leadership will maintain, validate, and approve deterioration scoring thresholds and escalation rules.

### 3.4 Constraints

- **Regulatory Compliance** — The redesign must comply with all applicable clinical safety, privacy, and regulatory standards.

- **EMR Integration Limitations** — The EWS must operate within existing EMR integration capabilities and cannot exceed current API, data-exchange, or system-bandwidth limits.

- **Threshold & Escalation Governance** — Scoring thresholds and escalation rules are governed by clinical leadership and cannot be modified arbitrarily.

- **Infrastructure Capacity** — The solution must function within existing hospital network, server, and device capacity.

- **Role & Workflow Boundaries** — The redesign cannot redefine clinical roles or expand responsibilities beyond established ICU practice.

### 3.5 Dependencies

- **EMR Team Deliverables** — The redesign depends on EMR team support, including API access, stable data feeds, and configuration assistance.

- **Clinical Leadership Approvals** — The project depends on clinical leadership to review and approve scoring thresholds, governance rules, escalation pathways, and workflow sign-off.

- **IT Infrastructure Support** — The redesign depends on IT teams to ensure server access, network reliability, and system monitoring remain stable.

- **Training & Change Management** — The project depends on training teams to deliver updated EWS workflow training to ICU staff.

- **Device Data Availability** — The project depends on automated vital-sign devices continuing to transmit complete and reliable data to the EMR/EWS.

## 4. Current-State & Future-State Process Architecture

### 4.1 Current-State Overview (As-Is)

The current ICU Early Warning System workflow begins with the nurse manually recording patient vitals and entering them into the EWS. This single human-dependent entry point introduces variability and timing delays that propagate downstream. Once vitals are submitted, the system automatically calculates an Early Warning Score and compares it against established deterioration thresholds. This decision point determines whether the workflow continues: if the score is below threshold, the process ends with no further action; if it exceeds threshold, the system generates an alert that is routed only to the clinician.

Upon receiving the alert, the clinician reviews the data, validates the patient’s condition, and decides whether intervention is required. If no intervention is needed, the clinician documents the assessment and the workflow terminates. If intervention is required, the clinician initiates the appropriate response and communicates the decision to the ICU coordinator. This dependency creates a visibility gap in which the ICU coordinator only becomes aware of the alert after the clinician has already decided to intervene. Once notified, the coordinator logs the escalation and updates the ICU dashboard, marking the end of the workflow.

Overall, the As-Is workflow is characterized by:

- Manual vitals entry as the single point of initiation
- Sequential alert routing (clinician → coordinator)
- High dependency on clinician validation before any coordination begins
- Limited situational awareness for the ICU coordinator
- Latency introduced at multiple human-dependent steps

### 4.2 Future-State Overview (To-Be)

In the Future-State design, manually recorded vitals from the nurse are consolidated with the automated vitals ingestion stream before entering the EWS scoring algorithm. This dual-source model stabilizes data frequency and reduces timing variability by ensuring that both manual and automated inputs flow through a unified ingestion process.

Once vitals are consolidated, the system enriches the alert with additional clinical context and severity indicators to support faster and more informed clinician triage. A key architectural improvement in the redesign is parallel alert routing, where the enriched alert is sent simultaneously to both the clinician and the ICU coordinator. This eliminates the visibility gap present in the Current-State workflow and enables the coordinator to prepare for potential intervention earlier.

By reducing human-dependent timing variations and improving shared situational awareness, the Future-State workflow introduces greater predictability, stronger coordination, and fewer delays across the ICU team.

### 4.3 Key Workflow Changes

By transforming the Current-State into the Future-State workflow, several structural changes directly address the bottlenecks identified earlier. First, automated vitals ingestion is consolidated with existing manual vitals entry, creating a unified and more reliable data input process. This reduces timing variability and minimizes human-dependent delays at the point of initiation.

Second, the alert is enriched with additional clinical context and severity categorization. This enhancement alleviates a major bottleneck in the clinician triage workflow by reducing the amount of manual information gathering required before making an intervention decision.

Third, routing the enriched alert in parallel to both the clinician and the ICU coordinator eliminates the visibility gap present in the Current-State. This parallel routing reduces intervention preparation time and improves overall coordination, ultimately raising the level of medical service delivery.

## 5. Business & Functional Requirements

### 5.1 Business Rules

- **Vitals Consolidation Rule** — All manually recorded vitals must be combined with automated vitals ingestion before any Early Warning Score is calculated.

- **Timestamp Integrity Rule** — All vitals data must be timestamped and validated for completeness prior to scoring.

- **Clinical Context Rule** — All high-severity alerts must include sufficient clinical context for safe and informed clinician triage.

- **Threshold Escalation Rule** — Any Early Warning Score that exceeds the defined clinical threshold must trigger a severity-categorized alert.

- **Parallel Notification Rule** — All high-severity alerts must be sent to both the clinician and ICU coordinator simultaneously to ensure shared situational awareness.

- **Clinician Validation Rule** — Clinicians must validate the underlying vital data associated with an alert before initiating any intervention.

- **Coordinator Accountability Rule** — ICU coordinators must have visibility into which clinician is assigned to each high-severity alert.

- **Clinical Documentation Rule** — Clinicians must document whether the alert was true or false and record all clinical judgments following any intervention.

- **Escalation Logging Rule** — ICU coordinators must log the intervention time, responsible clinician, and nature of the intervention for system-wide reporting.

- **Workflow Termination Rule** — The alert workflow formally ends only when the alert is documented as false or when post-intervention notes are completed.

### 5.2 Functional Requirements (MoSCoW)

#### ⭐ Must-Have Functional Requirements

- **Unified vitals ingestion** — The system must ingest manual and automated vitals into a unified, consolidated data stream before scoring.

- **Vitals validation & timestamping** — The system must validate vitals for completeness and timestamp accuracy, and flag missing or anomalous data for clinical review.

- **Continuous scoring engine** — The system must continuously calculate the Early Warning Score based on the incoming vitals stream.

- **Alert enrichment behavior** — The system must enrich alerts with relevant clinical context, including patient history, vitals trends, and severity categorization.

- **Parallel alert routing** — The system must route high-severity alerts to both the clinician and ICU coordinator simultaneously and record alert receipt timestamps.

- **Clinician validation workflow** — The system must require clinicians to validate whether the alert is true or false before initiating intervention.

- **Documentation capture — clinician** — The system must allow clinicians to document assessment results, intervention actions, timestamps, and supporting evidence.

- **Coordinator visibility** — The system must display clinician assignment and alert status so ICU coordinators have real-time visibility.

- **Escalation logging** — The system must log intervention time, responsible clinician, nature of intervention, and final resolution.

- **Workflow closure logic** — The system must close the alert workflow only after clinician validation, intervention documentation, and coordinator logging are completed.

#### ⭐ Should-Have Functional Requirements

- **Vitals trend visualization** — The system should provide vitals trend visualizations and basic analytical tools to help clinicians interpret deterioration patterns and clinical context.

- **Severity color-coding** — The system should display 3–5 severity levels using standardized color-coding, each with an associated recommended response time window.

- **Coordinator dashboard enhancements** — The system should display clinician shift schedules and availability to help ICU coordinators manage alert assignments during rotating shifts.

#### ⭐ Could-Have Functional Requirements

- **Predictive analytics** — The system could provide forecast-oriented analytics to highlight potential deterioration trends before Early Warning Score thresholds are triggered.

- **Mobile alert notifications** — The system could send alerts to clinicians’ mobile devices outside standard communication channels to improve response time during high-severity events.

### 5.3 Non-Functional Requirements (NFRs)

- **System performance** — The system should generate alerts and update workflow states with minimal latency to ensure timely clinical response.

- **Reliability & uptime** — The system should maintain high availability to support continuous ICU operations without interruption.

- **Security & privacy** — The system should enforce strict access controls and protect all patient information in alignment with clinical privacy standards.

- **Auditability** — The system should maintain a complete audit trail of alert generation, validation, intervention, and closure for compliance and review.

- **Scalability** — The system should scale to support increased patient volume or ICU expansion without degrading performance.

- **Usability** — The system should present information in a clear, intuitive format so clinicians and coordinators can operate it efficiently under time-sensitive conditions.

- **Interoperability** — The system should integrate smoothly with existing hospital systems and vitals monitoring devices to ensure consistent data flow.

## 6. System Integration & Data Touchpoints

The ICU Early Warning System (EWS) relies on a set of interconnected components that work together to capture patient vitals, consolidate data, compute deterioration scores, generate enriched alerts, and support coordinated clinical response. This section outlines the system interfaces, data flow, and role-based access considerations that enable the future-state workflow to operate reliably, safely, and with reduced latency.

### 6.1 System Interfaces

The ICU EWS depends on several system-to-system interfaces that ensure data moves reliably through the workflow. These interfaces support continuous vitals ingestion, deterioration scoring, alert generation, clinician validation, and documentation.

#### Participating Systems

- Vitals Monitoring Devices — Capture continuous automated vital signs.
- Nurse Input — Provides manually recorded vitals and observational notes.
- Electronic Medical Record (EMR) — Centralized repository and single source of truth.
- EWS Scoring Engine — Computes deterioration scores and generates alerts.
- Clinician Dashboard — Displays enriched alerts requiring validation or intervention.
- ICU Coordinator Dashboard — Provides parallel visibility into alert status and escalation readiness.

#### Interface Overview

##### Vitals Monitoring Devices → EMR

**Workflow Alignment:** Continuous automated vitals ingestion supports early deterioration detection and ensures real-time visibility into patient status. Centralizing device-captured vitals in the EMR eliminates manual delays and strengthens reliability through a single source of truth.

**Integration Improvements:** Fixes fragmented vitals ingestion. Introduces continuous streaming, reducing clinical risk from outdated data and improving scoring accuracy.

##### Nurse → EMR

**Workflow Alignment:** Manually captured vitals and observations are timestamped and integrated alongside automated measurements, ensuring data completeness. This supports continuous scoring and reduces blind spots in patient monitoring.

**Integration Improvements:** Fixes inconsistent manual documentation. Introduces standardized, unified data entry that reduces clinical risk and reinforces EMR reliability.

##### EMR → EWS Scoring Engine

**Workflow Alignment:** Consolidated vitals feed directly into the scoring engine, enabling continuous computation of deterioration scores. High-quality, validated inputs improve alert accuracy and reduce false positives.

**Integration Improvements:** Fixes fragmented data sources and slow scoring cycles. Introduces unified, continuous data flow that reduces clinical risk through faster detection.

##### EWS Scoring Engine → Clinician Dashboard

**Workflow Alignment:** Severity-categorized, enriched alerts are delivered immediately when thresholds are exceeded, supporting rapid clinician validation and intervention.

**Integration Improvements:** Fixes delayed or ambiguous alerts. Introduces enriched, actionable notifications that improve responsiveness and clarity.

##### EWS Scoring Engine → ICU Coordinator Dashboard

**Workflow Alignment:** Alerts are routed to the ICU coordinator in parallel with clinicians, enabling proactive preparation and improving coordination.

**Integration Improvements:** Fixes coordinator visibility delays. Introduces parallel routing that shortens escalation timelines and improves response quality.

##### Clinician Dashboard → EMR

**Workflow Alignment:** Clinician validations, intervention details, and supporting evidence are formally documented in the EMR, closing the alert workflow loop.

**Integration Improvements:** Fixes inconsistent intervention documentation. Introduces structured, enforced documentation that strengthens patient safety and auditability.

### 6.2 Data Flow Overview

#### High-Level Summary

Data originates from the patient’s physiological condition through automated device streams and manual nurse entries. These sources are consolidated in the EMR, establishing a single source of truth. The scoring engine extracts relevant vitals to compute deterioration scores, generate enriched alerts, and categorize severity. After clinicians validate alerts and perform interventions, the EMR is updated with their decisions and supporting evidence, completing the workflow loop.

#### Major Data Flow Stages

- Data Capture
- Data Consolidation
- Data Scoring
- Alert Generation
- Alert Routing
- Clinical Validation
- Intervention Documentation
- Workflow Closure

#### Data Transformation at Each Stage

Raw vitals become timestamped structured data; consolidated vitals become algorithm-ready inputs; numerical scores become enriched alerts; enriched alerts become clinical decisions; clinical decisions become formal EMR documentation.

#### Workflow Alignment

Centralizing vitals in the EMR ensures consistency and reliability. Timestamped vitals improve scoring accuracy. Parallel alert routing enhances responsiveness by giving coordinators early visibility. These improvements eliminate nurse latency, clinician decision delays, and coordinator visibility gaps.

#### Future-State Improvements

The redesigned data flow reduces latency, improves data accuracy, and strengthens responsiveness. Automated ingestion removes timing variability, enriched alerts reduce cognitive load, and parallel routing ensures coordinators can prepare earlier. Together, these enhancements improve clinical quality through faster detection, clearer alerting, and reliable documentation.

### 6.3 Role-Based Access Considerations

Role-based access ensures each participant interacts only with the data and actions required for their responsibilities, enhancing safety, accountability, and data integrity.

#### Nurse Access

Nurses can view EMR data relevant to direct patient care and enter or edit the vital signs they are responsible for capturing. Their access is limited to vitals and observational inputs to minimize data corruption risk. They can see downstream workflow activities only when relevant to their operational responsibilities.

#### Clinician Access

Clinicians have full visibility into the patient’s EMR, excluding irrelevant personal information. They are the only role qualified to validate alerts. Clinicians can document validation decisions, intervention actions, and supporting evidence. Their ability to modify EMR data is restricted to scenarios tied to alerts or formal clinical documentation.

#### ICU Coordinator Access

Coordinators can view EMR data relevant to deterioration scoring and alert status, supporting proactive intervention preparation. They can log coordination activities but cannot override or dismiss alerts, ensuring deterioration signals are never suppressed.

#### Workflow Safety Alignment

These access boundaries prevent unauthorized data changes, reduce incorrect alert handling, and ensure each role operates within its defined scope. They eliminate current-state bottlenecks such as nurse-dependent delays, clinician ambiguity, and coordinator visibility gaps.

#### Future-State Improvements

The redesigned access model ensures each role contributes only the data they are responsible for while receiving the information necessary to act quickly and accurately. Parallel routing, enriched alert visibility, and structured documentation rely on these access rules to function correctly.

#### Access Philosophy Summary

Each role has access only to the data and actions required to perform their part of the workflow, ensuring safety, clarity, and accountability across the ICU EWS process.
