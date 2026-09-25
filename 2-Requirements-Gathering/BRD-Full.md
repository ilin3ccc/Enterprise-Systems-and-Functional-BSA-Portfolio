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

## 7. Traceability & Acceptance Criteria

### 7.1 Requirements Traceability Matrix

#### Business Rules

<table>
  <tr>
    <th>Req ID</th>
    <th>Type</th>
    <th>Source</th>
    <th>Requirement Description</th>
    <th>Future-State System Behavior</th>
    <th>TC ID</th>
    <th>AC ID</th>
  </tr>

  <tr>
    <td>REQ-BR-001</td>
    <td>Business Rule</td>
    <td>Business Rules</td>
    <td>Vitals must be consolidated from manual and automated sources.</td>
    <td>System consolidates manual and automated vital streams into a unified dataset in the EMR before scoring.</td>
    <td>TC-001</td>
    <td>AC-01</td>
  </tr>

  <tr>
    <td>REQ-BR-002</td>
    <td>Business Rule</td>
    <td>Business Rules</td>
    <td>All vitals must include accurate timestamps; missing timestamps must be flagged.</td>
    <td>System validates timestamps for each vitals entry and flags missing values for nurse correction or sign-off.</td>
    <td>TC-002</td>
    <td>AC-02</td>
  </tr>

  <tr>
    <td>REQ-BR-003</td>
    <td>Business Rule</td>
    <td>Business Rules</td>
    <td>Alerts must include severity code and clinical context.</td>
    <td>System attaches severity codes to alerts and enriches them with relevant clinical context and quick-access tools.</td>
    <td>TC-003</td>
    <td>AC-03</td>
  </tr>

  <tr>
    <td>REQ-BR-004</td>
    <td>Business Rule</td>
    <td>Business Rules</td>
    <td>Severity thresholds must trigger escalation.</td>
    <td>System applies severity categories to alerts and enforces escalation rules based on the assigned urgency level.</td>
    <td>TC-004</td>
    <td>AC-04</td>
  </tr>

  <tr>
    <td>REQ-BR-005</td>
    <td>Business Rule</td>
    <td>Business Rules</td>
    <td>Alerts must be routed in parallel to clinician and coordinator.</td>
    <td>System routes alerts in parallel to both clinician and coordinator and verifies successful delivery to each role.</td>
    <td>TC-005</td>
    <td>AC-05</td>
  </tr>

  <tr>
    <td>REQ-BR-006</td>
    <td>Business Rule</td>
    <td>Business Rules</td>
    <td>Clinician must validate alert before workflow closure.</td>
    <td>System flags alerts lacking clinician validation and surfaces them for leadership review at defined intervals.</td>
    <td>TC-006</td>
    <td>AC-06</td>
  </tr>

  <tr>
    <td>REQ-BR-007</td>
    <td>Business Rule</td>
    <td>Business Rules</td>
    <td>Coordinator must acknowledge and track escalations.</td>
    <td>System requires coordinator acknowledgment for each escalation and flags delays for leadership review.</td>
    <td>TC-007</td>
    <td>AC-07</td>
  </tr>

  <tr>
    <td>REQ-BR-008</td>
    <td>Business Rule</td>
    <td>Business Rules</td>
    <td>Clinician must document intervention details.</td>
    <td>System keeps alerts open and marked incomplete until clinician intervention details are documented.</td>
    <td>TC-008</td>
    <td>AC-08</td>
  </tr>

  <tr>
    <td>REQ-BR-009</td>
    <td>Business Rule</td>
    <td>Business Rules</td>
    <td>All escalations must be logged.</td>
    <td>System maintains a dedicated record for each alert and logs all escalation and resolution events for review.</td>
    <td>TC-009</td>
    <td>AC-09</td>
  </tr>

  <tr>
    <td>REQ-BR-010</td>
    <td>Business Rule</td>
    <td>Business Rules</td>
    <td>Workflow ends only after validation + documentation.</td>
    <td>System keeps escalation workflows open and marked incomplete until clinician validation and documentation are submitted.</td>
    <td>TC-010</td>
    <td>AC-10</td>
  </tr>

  <tr>
    <td>REQ-BR-011</td>
    <td>Business Rule</td>
    <td>Business Rules</td>
    <td>Alerts must persist until resolved; no auto-dismissal.</td>
    <td>System restricts alert dismissal to clinical leadership and requires documentation and rationale for closure.</td>
    <td>TC-011</td>
    <td>AC-11</td>
  </tr>

</table>

#### Must-Have Functional Requirements

<table>
  <tr>
    <th>Req ID</th>
    <th>Type</th>
    <th>Source</th>
    <th>Requirement Description</th>
    <th>Future-State System Behavior</th>
    <th>TC ID</th>
    <th>AC ID</th>
  </tr>

  <tr>
    <td>REQ-MH-001</td>
    <td>Must-Have</td>
    <td>Functional Requirements</td>
    <td>System must ingest manual + automated vitals into unified stream.</td>
    <td>System consolidates manual and automated vital streams into a unified dataset before scoring.</td>
    <td>TC-012</td>
    <td>AC-12</td>
  </tr>

  <tr>
    <td>REQ-MH-002</td>
    <td>Must-Have</td>
    <td>Functional Requirements</td>
    <td>System must validate vitals and reject corrupted entries.</td>
    <td>System flags vitals anomalies and restricts data edits to leadership-approved corrections.</td>
    <td>TC-013</td>
    <td>AC-13</td>
  </tr>

  <tr>
    <td>REQ-MH-003</td>
    <td>Must-Have</td>
    <td>Functional Requirements</td>
    <td>System must continuously calculate EWS scores.</td>
    <td>System monitors its operational status continuously and flags outages for leadership and IT review.</td>
    <td>TC-014</td>
    <td>AC-14</td>
  </tr>

  <tr>
    <td>REQ-MH-004</td>
    <td>Must-Have</td>
    <td>Functional Requirements</td>
    <td>System must enrich alerts with severity, context, timestamps.</td>
    <td>System verifies alert severity, context, and timestamps before initiating routing.</td>
    <td>TC-015</td>
    <td>AC-15</td>
  </tr>

  <tr>
    <td>REQ-MH-005</td>
    <td>Must-Have</td>
    <td>Functional Requirements</td>
    <td>System must route alerts in parallel.</td>
    <td>System routes alerts in parallel to clinician and coordinator and confirms receipt for both roles.</td>
    <td>TC-016</td>
    <td>AC-16</td>
  </tr>

  <tr>
    <td>REQ-MH-006</td>
    <td>Must-Have</td>
    <td>Functional Requirements</td>
    <td>System must allow clinician validation.</td>
    <td>System restricts alert validation to clinicians and records each validation action.</td>
    <td>TC-017</td>
    <td>AC-17</td>
  </tr>

  <tr>
    <td>REQ-MH-007</td>
    <td>Must-Have</td>
    <td>Functional Requirements</td>
    <td>System must capture clinician documentation.</td>
    <td>System keeps alerts open and flagged until required clinician documentation is submitted.</td>
    <td>TC-018</td>
    <td>AC-18</td>
  </tr>

  <tr>
    <td>REQ-MH-008</td>
    <td>Must-Have</td>
    <td>Functional Requirements</td>
    <td>System must provide coordinator visibility.</td>
    <td>System ensures coordinator visibility by confirming coordinator receipt of all active alerts.</td>
    <td>TC-019</td>
    <td>AC-19</td>
  </tr>

  <tr>
    <td>REQ-MH-009</td>
    <td>Must-Have</td>
    <td>Functional Requirements</td>
    <td>System must log escalations.</td>
    <td>System maintains a dedicated record for each alert and logs all escalation and resolution events.</td>
    <td>TC-020</td>
    <td>AC-20</td>
  </tr>

  <tr>
    <td>REQ-MH-010</td>
    <td>Must-Have</td>
    <td>Functional Requirements</td>
    <td>System must close workflow only after validation + documentation.</td>
    <td>System prevents alert closure until clinician validation and required documentation are submitted.</td>
    <td>TC-021</td>
    <td>AC-21</td>
  </tr>

</table>

#### Should-Have Functional Requirements

<table>
  <tr>
    <th>Req ID</th>
    <th>Type</th>
    <th>Source</th>
    <th>Requirement Description</th>
    <th>Future-State System Behavior</th>
    <th>TC ID</th>
    <th>AC ID</th>
  </tr>

  <tr>
    <td>REQ-SH-001</td>
    <td>Should-Have</td>
    <td>Functional Requirements</td>
    <td>System should display vitals trend visualization.</td>
    <td>System provides visual trend representations of vitals to support clinician interpretation during triage.</td>
    <td>TC-022</td>
    <td>AC-22</td>
  </tr>

  <tr>
    <td>REQ-SH-002</td>
    <td>Should-Have</td>
    <td>Functional Requirements</td>
    <td>System should color-code alerts by severity.</td>
    <td>System applies color-coding to severity categories to enhance alert clarity and prioritization.</td>
    <td>TC-023</td>
    <td>AC-23</td>
  </tr>

  <tr>
    <td>REQ-SH-003</td>
    <td>Should-Have</td>
    <td>Functional Requirements</td>
    <td>System should enhance coordinator dashboard with filters.</td>
    <td>System enhances the coordinator dashboard by providing alert filtering options for faster triage.</td>
    <td>TC-024</td>
    <td>AC-24</td>
  </tr>

</table>

#### Could-Have Functional Requirements

<table>
  <tr>
    <th>Req ID</th>
    <th>Type</th>
    <th>Source</th>
    <th>Requirement Description</th>
    <th>Future-State System Behavior</th>
    <th>TC ID</th>
    <th>AC ID</th>
  </tr>

  <tr>
    <td>REQ-CH-001</td>
    <td>Could-Have</td>
    <td>Functional Requirements</td>
    <td>System could provide predictive analytics.</td>
    <td>System integrates predictive analytics into its scoring logic to provide advanced deterioration warnings.</td>
    <td>TC-025</td>
    <td>AC-25</td>
  </tr>

  <tr>
    <td>REQ-CH-002</td>
    <td>Could-Have</td>
    <td>Functional Requirements</td>
    <td>System could send mobile alert notifications.</td>
    <td>System incorporates mobile alert notifications into its routing process to reduce response delays.</td>
    <td>TC-026</td>
    <td>AC-26</td>
  </tr>

</table>

#### Non-Functional Requirements (NFRs)

<table>
  <tr>
    <th>Req ID</th>
    <th>Type</th>
    <th>Source</th>
    <th>Requirement Description</th>
    <th>Future-State System Behavior</th>
    <th>TC ID</th>
    <th>AC ID</th>
  </tr>

  <tr>
    <td>REQ-NFR-001</td>
    <td>NFR</td>
    <td>System Integration</td>
    <td>Dashboard must update within 5 seconds.</td>
    <td>System enforces a 5-second alert update window by comparing generation and display timestamps and flagging deviations for IT review.</td>
    <td>TC-027</td>
    <td>AC-27</td>
  </tr>

  <tr>
    <td>REQ-NFR-002</td>
    <td>NFR</td>
    <td>System Integration</td>
    <td>System must maintain 99.9% uptime.</td>
    <td>System maintains 99.9% uptime and exposes uptime metrics for continuous IT monitoring.</td>
    <td>TC-028</td>
    <td>AC-28</td>
  </tr>

  <tr>
    <td>REQ-NFR-003</td>
    <td>NFR</td>
    <td>System Integration</td>
    <td>System must be usable with minimal training.</td>
    <td>System reinforces usability by providing a minimalist interface that highlights essential elements and reduces cognitive load.</td>
    <td>TC-029</td>
    <td>AC-29</td>
  </tr>

  <tr>
    <td>REQ-NFR-004</td>
    <td>NFR</td>
    <td>System Integration</td>
    <td>System must interoperate with EMR and devices.</td>
    <td>System interoperates with the EMR and vitals monitoring devices through a unified data pipeline that supports seamless information exchange.</td>
    <td>TC-030</td>
    <td>AC-30</td>
  </tr>

</table>

### 7.2 Acceptance Criteria

Below are the acceptance criteria for all requirements in Section 7.1. Each requirement includes two testable acceptance criteria, written in clear, unambiguous language.

#### Business Rules

##### REQ-BR-001 — Vitals Consolidation

- System successfully merges manual and automated vitals into a unified dataset.
- Scoring engine only initiates after consolidation is complete.

##### REQ-BR-002 — Timestamp Integrity

- System flags vitals entries missing timestamps.
- System prevents scoring until timestamps are corrected or signed off.

##### REQ-BR-003 — Severity + Context Enrichment

- System attaches severity codes to all alerts.
- System displays relevant clinical context alongside each alert.

##### REQ-BR-004 — Threshold Escalation

- Alerts escalate according to predefined severity thresholds.
- System enforces urgency rules consistently across all alert types.

##### REQ-BR-005 — Parallel Routing

- Alerts are routed simultaneously to clinician and coordinator.
- System confirms successful delivery to both roles.

##### REQ-BR-006 — Clinician Validation

- System prevents workflow closure until clinician validation is recorded.
- System flags unvalidated alerts for leadership review.

##### REQ-BR-007 — Coordinator Accountability

- System requires coordinator acknowledgment for escalations.
- System flags delayed acknowledgments for leadership review.

##### REQ-BR-008 — Intervention Documentation

- System keeps alerts open until intervention documentation is submitted.
- System marks alerts incomplete when documentation is missing.

##### REQ-BR-009 — Escalation Logging

- System logs every escalation event with timestamp and role.
- System maintains a complete history of alert progression.

##### REQ-BR-010 — Workflow Termination

- System prevents alert closure until validation and documentation are complete.
- System marks alerts incomplete if either requirement is missing.

##### REQ-BR-011 — Alert Persistence

- System restricts alert dismissal to clinical leadership.
- System requires documentation and rationale for dismissal.

#### Must-Have Functional Requirements

##### REQ-MH-001 — Unified Vitals Ingestion

- System ingests manual and automated vitals into a unified stream.
- Scoring engine only runs after ingestion is complete.

##### REQ-MH-002 — Vitals Validation

- System flags anomalies or corrupted vitals entries.
- System restricts edits to leadership-approved corrections.

##### REQ-MH-003 — Continuous Scoring Engine

- System monitors operational status continuously.
- System flags outages for IT and leadership review.

##### REQ-MH-004 — Alert Verification

- System verifies severity, context, and timestamps before routing.
- System blocks routing if required alert metadata is missing.

##### REQ-MH-005 — Parallel Routing Confirmation

- System routes alerts in parallel to clinician and coordinator.
- System confirms receipt for both roles before workflow continues.

##### REQ-MH-006 — Clinician Validation

- System restricts validation actions to clinicians.
- System records each validation event with timestamp.

##### REQ-MH-007 — Documentation Capture

- System keeps alerts flagged until documentation is submitted.
- System prevents workflow closure when documentation is missing.

##### REQ-MH-008 — Coordinator Visibility

- System confirms coordinator receipt of all active alerts.
- System displays all active alerts on coordinator dashboard.

##### REQ-MH-009 — Escalation Logging

- System creates a dedicated record for each alert.
- System logs all escalation and resolution events.

##### REQ-MH-010 — Workflow Closure Logic

- System prevents alert closure until validation and documentation are complete.
- System marks alerts incomplete when either requirement is missing.

#### Should-Have Functional Requirements

##### REQ-SH-001 — Vitals Trend Visualization

- System displays vitals trends in graphical format.
- Clinicians can interpret trends during triage without additional tools.

##### REQ-SH-002 — Severity Color Coding

- System applies color coding to severity categories.
- Color coding is visible across all alert views.

##### REQ-SH-003 — Coordinator Dashboard Filters

- System provides filtering options on the coordinator dashboard.
- Filters allow faster triage and prioritization.

#### Could-Have Functional Requirements

##### REQ-CH-001 — Predictive Analytics

- System generates predictive deterioration indicators.
- Predictive warnings appear alongside standard alerts.

##### REQ-CH-002 — Mobile Notifications

- System sends mobile notifications as part of alert routing.
- Mobile notifications reduce response delays.

#### Non-Functional Requirements (NFRs)

##### REQ-NFR-001 — Performance (5-second update)

- System updates dashboard within 5 seconds of alert creation.
- System flags update delays for IT review.

##### REQ-NFR-002 — Reliability (99.9% uptime)

- System maintains 99.9% uptime.
- System exposes uptime metrics for continuous monitoring.

##### REQ-NFR-003 — Usability

- System interface highlights essential elements only.
- Users can operate the system with minimal training.

##### REQ-NFR-004 — Interoperability

- System exchanges data seamlessly with EMR and vitals devices.
- Unified data pipeline supports consistent information flow.
