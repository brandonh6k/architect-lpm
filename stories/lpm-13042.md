# LTE VoWiFi Enhancements
> Enable processing and visualization of Voice over WiFi location data for LTE calls

## Business Context
With the increasing ubiquity of WiFi access points, a growing percentage of emergency calls are being routed over WiFi networks. Currently, VoWiFi location data is not integrated into the LPM system, creating gaps in location analytics and reporting capabilities. Since WiFi locations typically provide low-uncertainty, high-confidence positioning data, integrating this data will significantly improve carriers' FCC compliance reporting. This enhancement will:

- Support compliance with emergency services location requirements
- Provide consistent and consolidated location metrics across both cellular and WiFi-based calls
- Improve overall location reporting accuracy through high-confidence WiFi position data
- Enable comprehensive analytics across all emergency call types

## Epic Goal
To enhance LPM's capability to process, store, and visualize Voice over WiFi location data from LTE calls, enabling better location tracking and reporting for VoWiFi emergency calls.

## User Impact
- Primary users: LPM system users
- Current experience: VoWiFi location data is not currently ingested into the LPM system
- Desired experience: Users can access and analyze VoWiFi location data through existing LPM interfaces including dashboards, reports, and finder tools

## Scope
### Key Features & Capabilities
- Parse and store VoWiFi data from the ESPOSRSP TDR Audit stream
- Match ESPOSRSP records with LTETDR records using trid and timestamp
- Create position records from matched data with Pos Method of DL
- Integrate VoWiFi data into Location Finder, 4RPO ETL and RunBI
- Add VoWiFi data to Reports 
- Include VoWiFi metrics in LPM Dashboard and KPI screens

### Out of Scope
- 5G VoWiFi enhancements (covered in separate epic - LPM-13215) 
- Changes to existing MoveIT file delivery mechanism
- Modifications to source data format

## Dependencies & Constraints
- Systems impacted: LPM Database, 4RPO ETL processes, Reports, Dashboard, Finder tools
- External dependencies:
  - ESPOSRSP TDR Audit feed from Metrics team (SBR-1784 - complete)
  - Daily file uploads to LPM MoveIT account (complete)
- Technical constraints:
  - Need to handle non-unique trid values
  - Must support fuzzy timestamp matching
  - Must handle out-of-order file processing

## Initial Story Breakdown
1. LPM-11503: Parse and Store LTE VoWiFi TDR and TDR Audit Data
2. LPM-11504: Incorporate LTE VoWiFi data into Reports
3. LPM-11505: Incorporate LTE VoWiFi data into Location Finder
4. LPM-11507: Incorporate LTE VoWiFi into LPM Dashboard

## Risks & Assumptions
- Assumption: ESPOSRSP files will be delivered daily
- Risk: Timing mismatches between TDR and ESPOSRSP file delivery
- Risk: Performance impact of multiple matching attempts
- Risk: Data quality issues in source files

## Additional Information
- Related epics: LPM-13215 (5G VoWiFi Enhancements)
- Helpful resources: ESPOSRSP file format documentation
- Background documents: SBR-1784 documentation

---
Size Estimate: L
Priority: High