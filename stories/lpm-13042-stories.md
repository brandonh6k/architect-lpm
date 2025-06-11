# LPM-13042 User Stories

## LPM-11503: Parse and Store LTE VoWiFi TDR and TDR Audit Data

**As a** LPM System
**I want to** ingest and process VoWiFi location data from ESPOSRSP TDR Audit files
**So that** the location data can be matched with TDR records, generate location records and made available for analysis

### Acceptance Criteria
1. System can parse ESPOSRSP CSV files with the following columns:
   - ctid, trid, comm_ts, rmtne_type, rmtne_systemid, transid
   - position_latitude, position_longitude, position_horiz_uncert
   - position_conf, position_alt, position_alt_uncert
   - position_gen_time, mdn, min

2. Records are stored in new ESPOSRSP table in LPM database.  All records are imported into all instances (both LTE and 5G) as there is no way to filter the data prior to matching to TDRs and the ESPOSRSP file is global across both technologies.
3. Implement matching process that:
   - Matches ESPOSRSP records with LTETDR records using trid and fuzzy timestamp
   - Runs after ESPOSRSP file ingestion
   - Runs after LTETDR file ingestion
   - Runs nightly before 4RPO ETL and RunBI processes
4. Creates position records for matched data with Pos Method of DL
5. Handles non-unique trid values appropriately
6. Processes files in any order

### Technical Notes
- Use existing MoveIT file delivery mechanism
- Store associated LTETDR key and timestamp for matched ESPOSRSP rows
- Position records go into standard LTE Position tables
- Matching window should be within -7 to +1 days of ESPOSRSP `comm_ts` timestamp.
- Each matching run will attempt to match any unmatched ESPOSRSP records from within the last 7 days.
- Mappings for location records from source data (defined below)
- Implement a manual CLI matching method.  Takes a date range and attempts to match any unmatched ESPOSRSP records in that range.  Defaults to last 7 days if a range isn't specified.

#### Location Record Creation:

This assumes we've got a matching LTETDR record.  If we don't, you shouldn't be here.

**Location Session:**

- LocationSessionTms = LTETDR.HDR_ALLOCATION_TIME

**Location Transaction:**

- LocationTransTms = LTETDR.HDR_ALLOCATION_TIME
- StartTms = LTETDR.HDR_ALLOCATION_TIME
- EndTms = ESPOSRSP.position_gen_time
- IMEI = LTETDR.MS_INFO_IMEI
- IMSI = LTETDR.MS_INFO_IMSI
- NetworkID1 = LTETDR.IMSLTE_CURRENT_CGI_MCC
- NetworkID2 = LTETDR.IMSLTE_CURRENT_CGI_MNC
- NetworkID3 = LTETDR.IMSLTE_CURRENT_CGI_LAC
- NetworkID4 = LTETDR.IMSLTE_CURRENT_CGI_CID
- Server: VoWiFi
- Node: VoWiFi

Notes:

- Need to make SourceID & SourceTms nullable as VoWiFi Locations won't have those values.
- Also add VoWiFi_ID & Tms columns and use them to tie to the underlying ESPOSRSP record.

**Location:**

- LocationTms = ESPOSRSP.position_gen_time
- PosMethod = DL
- EstLat = ESPOSRSP.position_latitude
- EstLon = ESPOSRSP.position_longitude
- Uncertainty = ESPOSRSP.position_horiz_uncert
- Altitude = ESPOSRSP.position_alt
- AltitudeUncert = ESPOSRSP.position_alt_uncert
- BarometricPressure = null
- Confidence = ESPOSRSP.position_conf
- StartTms = ESPOSRSP.comm_ts
- EndTms = ESPOSRSP.position_gen_time


---

## LPM-11504: Incorporate LTE VoWiFi data into Reports

**As a** LPM user
**I want to** see VoWiFi location data in system reports
**So that** I can analyze VoWiFi call locations and performance

### Acceptance Criteria
1. VoWiFi location data appears in existing reports
2. Reports properly identify DL position method where applicable
3. Reports include relevant VoWiFi metrics

### Technical Notes
- TODO: Define specific reports requiring updates - this should likely be none?

---

## LPM-11505: Incorporate LTE VoWiFi data into Location Finder and TDR Finder

**As a** LPM user
**I want to** search and view VoWiFi location data in the finder tools
**So that** I can investigate specific VoWiFi calls and locations

### Acceptance Criteria
1. VoWiFi records appear in Location Finder results
2. Location Finder displays raw ESPOSRSP data in table format when view log is clicked
3. Users can filter/search for VoWiFi records (likely by filtering by the DL pos method)

### Technical Notes
- Use table format similar to TDR Finder for raw data display
- Include all ESPOSRSP fields in raw data view

---

## LPM-11507: Incorporate LTE VoWiFi into LPM Dashboard

**As a** LPM user
**I want to** see VoWiFi metrics in the LPM Dashboard and existing KPI screens
**So that** I can monitor VoWiFi performance and trends

### Acceptance Criteria
1. VoWiFi data is included in relevant existing metrics

### Technical Notes
No explicit VoWiFi filter, but can filter to the VoWiFi "server"

---

## Dependencies
- ESPOSRSP TDR Audit feed (SBR-1784) - Complete
- Daily file uploads to MoveIT account - Complete

## Related Documentation
- ESPOSRSP file format documentation
- SBR-1784 documentation
