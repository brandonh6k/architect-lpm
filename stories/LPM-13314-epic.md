# Enable data access and streaming from EDG Datalake
> Enable near real-time data streaming and enhanced data access capabilities from EDG Datalake to support improved KPI functionality and customer management. This will be achieved by enabling Change Data Feed (CDF) on the `tdr_streaming` table in the Databricks Gold schema, streaming changes to an Azure Event Hub, and consuming those changes with Azure Stream Analytics to populate the appropriate LPM Azure SQL databases.

## Business Context
In 2025, EDG will have streaming KPI functionality between EDG and FMCC. Currently, data latency between these systems limits real-time insights and operational efficiency. Additionally, LPM has limited access to comprehensive data sets from FMCC, which restricts our ability to effectively onboard new customers independently and utilize richer data sources for enhanced analytics.

To address this, we will enable Change Data Feed (CDF) on the `tdr_streaming` table in the Databricks Gold schema. This will allow us to stream changes as they happen to an Azure Event Hub in the LPM environment. Azure Stream Analytics will then consume these changes, filter records based on `info_companyid` (or similar), and stream them into the appropriate LPM Azure SQL database. Event Hub and Stream Analytics resources will be provisioned in both Dev/Test and Prod environments.

## Epic Goal
Enable near real-time data streaming from EDG Datalake by leveraging Databricks CDF, Azure Event Hub, and Azure Stream Analytics, while providing LPM with enhanced data access capabilities to independently onboard customers, reduce data latency and leverage expanded data sets for improved operational insights. Ensure the solution is deployed and operational in both Dev/Test and Prod environments.

## User Impact
- Primary users: LPM operations team, LPM customers
- Current experience: Dependency on Metrics team for TDR feed generation (1-3 month backlog), limited data access with hourly/daily latency, restricted data sets
- Desired experience: Self-service customer TDR provisioning with no external dependencies, near real-time data insights, access to comprehensive data including Sipreq stanza for enhanced analytics
- New experience: Streaming data from Databricks to LPM Azure SQL databases via Event Hub and Stream Analytics, with records routed to the correct database based on company ID or similar logic. Improved agility and reduced latency for customer onboarding and near real-time analytics.

## Scope
### Key Features & Capabilities
- Implement near real-time data streaming from EDG Datalake using Databricks CDF on the `tdr_streaming` table
- Provision Azure Event Hub and Azure Stream Analytics resources in both Dev/Test and Prod environments
- Stream changes from Databricks to Event Hub as they occur
- Use Azure Stream Analytics to filter inbound records (e.g., by `info_companyid`) and route them to the appropriate LPM Azure SQL database
- Establish enhanced data access permissions and capabilities from FMCC
- Build support for independent customer management within LPM
- Enable access to expanded data sets (including Sipreq stanza)
- Integrate streaming KPI functionality between EDG and FMCC

### Out of Scope
- TDR audit records (will remain in existing flat-file workflows for this implementation)
- Migration of other existing flat-file based data ingestion workflows to streaming (only TDR data)
- Other data sources beyond base TDR data from EDG Datalake

## Success Metrics
- TDR data latency: Hourly/Daily SFTP flat files → Sub-minute streaming via Databricks CDF, Event Hub, and Stream Analytics (target: <60 seconds, stretch goal: <15 seconds)
- Data freshness: Batch processing → Near real-time continuous streaming from Databricks to Azure SQL
- Customer onboarding time: 1-3 months (Metrics team dependency) → Same-day self-service provisioning
- Successful provisioning and operation of Event Hub and Stream Analytics in both Dev/Test and Prod

## Dependencies & Constraints
- Systems impacted: EDG Datalake, FMCC, LPM, Databricks, Azure Event Hub, Azure Stream Analytics, Azure SQL
- External dependencies: EDG streaming KPI functionality completion in 2025
- Technical constraints: 
  - First streaming integration for LPM (all current workflows are flat-file based)
  - Unknown Databricks feature requirements and availability in Intrado's instance
  - Need to develop new streaming data ingestion patterns vs. existing file parsing workflows
  - Azure Event Hub and Stream Analytics must be provisioned and configured in both Dev/Test and Prod
  - Filtering and routing logic in Stream Analytics must be robust and maintainable
- Business constraints: [Need to identify]

## Initial Story / Task Breakdown
1. Assess current data access capabilities and limitations
2. Design streaming architecture for EDG to LPM data flow, including Databricks CDF, Event Hub, and Stream Analytics
3. Set up supporting Azure resources (Event Hub, Stream Analytics) in Dev/Test and Prod
4. Enable CDF on the `tdr_streaming` table in Databricks Gold schema
5. Implement streaming of changes from Databricks to Event Hub
6. Configure Stream Analytics to filter and route records to the correct LPM Azure SQL database
7. Testing and validation of near real-time data flows across all environments
8. Documentation for new streaming capabilities and operational procedures

## Risks & Assumptions
- Assumption: EDG streaming KPI functionality will be completed as planned in 2025
- Assumption: Required Databricks features (including CDF) will be available in Intrado's instance
- Assumption: Azure Event Hub and Stream Analytics can be provisioned and configured as required in both Dev/Test and Prod
- Risk: First streaming implementation may face significant learning curve and technical complexity
- Risk: Data access permissions may require extensive coordination with FMCC teams
- Risk: Existing flat-file based architecture may require substantial refactoring to support streaming
- Risk: Unknown Databricks feature requirements or Azure resource limitations could impact timeline or feasibility
- Risk: Sub-minute latency targets may not be achievable without significant infrastructure investment
- Risk: Filtering and routing logic in Stream Analytics may require tuning and ongoing maintenance