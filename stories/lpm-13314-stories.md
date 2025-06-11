# LPM-13314 User Stories

## LPM-13444: Assess current data access capabilities and limitations

**As a** solution architect
**I want to** understand the current data access methods, sources, and limitations
**So that** I can identify gaps and requirements for implementing real-time streaming from EDG Datalake

This story establishes a baseline understanding of how data currently moves through the system and what challenges exist. The findings will inform the design and implementation of the new streaming solution.

### Technical Notes
- Review documentation and interview key stakeholders
- Identify all current data sources, access methods, and pain points
- Summarize findings in a shareable document

### Acceptance Criteria
* Documented summary of current data access workflows, sources, and pain points
* Identification of key limitations and bottlenecks in the current process
* List of requirements for new streaming solution

---

## LPM-13445: Design streaming architecture for EDG to LPM data flow

**As a** solution architect
**I want to** design a streaming architecture using Databricks CDF, Azure Event Hub, and Azure Stream Analytics
**So that** we have a clear, scalable, and secure plan for real-time data movement from EDG Datalake to LPM Azure SQL

This story defines the technical approach for the new streaming pipeline, ensuring all requirements and constraints are addressed before implementation begins.

### Technical Notes
- Create architecture diagrams and data flow charts
- Specify filtering logic and security requirements
- Review design with stakeholders for feedback

### Acceptance Criteria
* Architecture diagram and documentation covering all major components
* Data flow and filtering logic defined (e.g., by `info_companyid`)
* Security, scalability, and operational considerations documented

---

## LPM-13446: Set up supporting Azure resources (Event Hub, Stream Analytics) in Dev/Test and Prod

**As a** DevOps engineer
**I want to** provision Azure Event Hub and Azure Stream Analytics in both Dev/Test and Prod
**So that** the streaming pipeline can be implemented and validated in all environments

Provisioning these resources is foundational for the streaming solution. Proper configuration and access control are critical for security and operational success.

### Technical Notes
- Use infrastructure-as-code (e.g., ARM, Bicep, or Terraform) where possible
- Ensure network, authentication, and RBAC are set up correctly
- Document resource names, endpoints, and access policies

### Acceptance Criteria
* Event Hub and Stream Analytics resources created in both Dev/Test and Prod
* Access and permissions configured for Databricks and LPM components
* Resource configuration documented

---

## LPM-13447: Enable CDF on the `tdr_streaming` table in Databricks Gold schema

**As a** Databricks admin
**I want to** enable Change Data Feed (CDF) on the `tdr_streaming` table
**So that** changes can be streamed in near real-time to downstream systems

Enabling CDF is a prerequisite for capturing and streaming incremental changes from Databricks. This step ensures the data pipeline can operate in real time.

### Technical Notes
- Review Databricks documentation for CDF prerequisites
- Enable CDF and validate with test data
- Document configuration steps and any caveats

### Acceptance Criteria
* CDF enabled and validated on the `tdr_streaming` table
* Documentation of CDF configuration and usage

---

## LPM-13448: Implement streaming of changes from Databricks to Event Hub

**As a** data engineer
**I want to** implement a process to stream changes from Databricks CDF to Azure Event Hub
**So that** all relevant changes are available for downstream processing in real-time

This story covers the development of the integration between Databricks and Azure Event Hub, ensuring reliable and performant data delivery.

### Technical Notes
- Use Databricks streaming APIs or notebooks to push changes to Event Hub
- Implement error handling and monitoring
- Validate throughput and latency

### Acceptance Criteria
* Streaming job/process implemented and running
* Data successfully flows from Databricks to Event Hub
* Monitoring and error handling in place

---

## LPM-13449: Configure Stream Analytics to filter and route records to the correct LPM Azure SQL database

**As a** data engineer
**I want to** configure Azure Stream Analytics to filter records (e.g., by `info_companyid`) and route them to the correct LPM Azure SQL database
**So that** each company’s data is delivered to the appropriate database

This story implements the logic to ensure each record is delivered to the correct destination, supporting multi-tenancy and data isolation.

### Technical Notes
- Define Stream Analytics query for filtering and routing
- Test with sample data for multiple companies
- Document query logic and output mappings

### Acceptance Criteria
* Stream Analytics job configured and running
* Filtering and routing logic validated
* Data appears in the correct Azure SQL database(s)

---

## LPM-13450: Testing and validation of near real-time data flows across all environments

**As a** QA engineer
**I want to** test and validate the end-to-end streaming pipeline in Dev/Test and Prod
**So that** we ensure data is delivered accurately, securely, and with low latency

Comprehensive testing is required to ensure the pipeline meets business and technical requirements. This story covers test planning, execution, and issue resolution.

### Technical Notes
- Develop test cases for all major scenarios (success, failure, edge cases)
- Validate data accuracy, latency, and security
- Track and resolve defects

### Acceptance Criteria
* Test cases and validation plan documented
* Successful test results for all major scenarios
* Issues and gaps identified and addressed

---

## LPM-13451: Documentation for new streaming capabilities and operational procedures

**As a** technical writer
**I want to** document the new streaming architecture, operational procedures, and troubleshooting steps
**So that** the team can operate and maintain the solution effectively

Clear and thorough documentation is essential for ongoing operations and support. This story ensures all relevant materials are created and reviewed.

### Technical Notes
- Document architecture, configuration, and operational procedures
- Include troubleshooting and escalation steps
- Review documentation with stakeholders

### Acceptance Criteria
* User and operator documentation completed
* Troubleshooting and escalation procedures documented
* Documentation reviewed and approved by stakeholders
