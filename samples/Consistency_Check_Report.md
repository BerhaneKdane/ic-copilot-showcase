# Consistency check - DEMO-GPF Gas Processing Facility (fictional)

> **STATUS: UNCHECKED - automated findings for engineer review. Not an approved deliverable.**

**Findings:** 39  critical 21 · major 15 · minor 3

## Findings

| # | Severity | Rule | Item | Finding | Clause | Source |
|---|---|---|---|---|---|---|
| 1 | critical | IC-012 | `300-PT-040A` | Trip setpoint for SIF-900-06 differs between documents: SIF register 150 barg, C&E 148 barg, alarm list 150 barg | FOC-SPC-IC-001 §8.1.4 |  |
| 2 | critical | IC-012 | `300-PT-040B` | Trip setpoint for SIF-900-06 differs between documents: SIF register 150 barg, C&E 148 barg, alarm list 150 barg | FOC-SPC-IC-001 §8.1.4 |  |
| 3 | critical | IC-014 | `200-LT-031` | System segregation: system: instrument 'SIS' vs io_point 'BPCS' | FOC-SPC-IC-001 §7.1.2 | instrument_index row 54 |
| 4 | critical | IC-014 | `800-GD-101C` | System segregation: system: instrument 'FGS' vs io_point 'SIS' | FOC-SPC-IC-001 §7.1.2 | instrument_index row 158 |
| 5 | critical | IC-015 | `100-PT-005` | Ex ia device allocated to non-IS channel CAB-BPCS-01/R1/S02/CH02 (AI-HART) | FOC-SPC-IC-001 §5.3.2 | io_list row 23 |
| 6 | critical | IC-017 | `200-BDV-043` | C&E effect references 200-BDV-043, which is not in the instrument index | FOC-SPC-IC-001 §7.3.1 | esd_cause_effect |
| 7 | critical | IC-020 | `SIF-900-04` | SIL 2 SIF uses 1oo1 initiators (HFT 0); minimum HFT 1 required | FOC-SPC-IC-001 §7.2.4 | sif_register row 8 |
| 8 | critical | IC-022 | `100-TT-007` | certified gas group IIA does not cover area group IIB | FOC-SPC-IC-001 §5.3.1 | instrument_index row 11 |
| 9 | critical | IC-022 | `400-BE-020B` | certified temperature class T2 does not cover area T3 | FOC-SPC-IC-001 §5.3.1 | instrument_index row 107 |
| 10 | critical | IC-025 | `300-PT-040B` | Redundant devices 300-PT-040A, 300-PT-040B share card CAB-SIS-01/R1/S01 | FOC-SPC-IC-001 §6.2.2 |  |
| 11 | critical | IC-025 | `400-BE-020B` | Redundant devices 400-BE-020A, 400-BE-020B share card CAB-SIS-01/R1/S05 | FOC-SPC-IC-001 §6.2.2 |  |
| 12 | critical | IC-026 | `300-PT-040B` | C&E cause for 300-PT-040B (SIF-900-06) does not action final element 300-ESDV-042 | FOC-SPC-IC-001 §7.3.1 |  |
| 13 | critical | IC-027 | `300-PT-044A` | C&E voting 2oo2 differs from SIF register 1oo2 for SIF-900-07 | FOC-SPC-IC-001 §7.3.2 | esd_cause_effect row 17 |
| 14 | critical | IC-027 | `300-PT-044B` | C&E voting 2oo2 differs from SIF register 1oo2 for SIF-900-07 | FOC-SPC-IC-001 §7.3.2 | esd_cause_effect row 18 |
| 15 | critical | IC-030 | `400-ZSC-204` | Sequence step references 400-ZSC-204, which is not in the instrument index | FOC-SPC-IC-001 §7.4.1 | bms_sequence row 8 |
| 16 | critical | IC-031 | `BMS-400 step 01` | PURGE time 120 s is below the minimum 300 s | FOC-SPC-IC-001 §7.4.2 | bms_sequence row 10 |
| 17 | critical | IC-031 | `BMS-400 step 03` | PILOT_TFI time 15 s exceeds the maximum 10 s | FOC-SPC-IC-001 §7.4.2 | bms_sequence row 12 |
| 18 | critical | IC-032 | `400-XV-023` | C&E effect E12 action 'CLOSE' does not match the safe state 'OPEN' in the instrument index (expected 'OPEN') | FOC-SPC-IC-001 §7.5.1 | esd_cause_effect column 21 |
| 19 | critical | IC-032 | `700-BDV-012` | C&E effect E15 action 'OPEN' does not match the safe state 'CLOSED' in the instrument index (expected 'CLOSE') | FOC-SPC-IC-001 §7.5.1 | esd_cause_effect column 24 |
| 20 | critical | IC-035 | `FG-300-FIRE` | Voting 1oo3 drives executive actions (300-ESDV-041, 300-ESDV-042, 300-XY-043, 700-BDV-013); at least 2ooN required | FOC-SPC-IC-001 §8.3.3 |  |
| 21 | critical | IC-036 | `FG-100-H2S-H` | Voting 2oo3 but only 2 initiator(s) listed in the F&G C&E | FOC-SPC-IC-001 §8.3.4 | F&G C&E: 800-GD-111A,800-GD-111B |
| 22 | major | IC-002 | `200-LT-012` | Tag '200-LT-012' appears 2 times (service: Separator condensate level, Separator condensate level (local indication)) | FOC-SPC-IC-001 §3.4.1 | instrument_index row 32; instrument_index row 35 |
| 23 | major | IC-003 | `CAB-BPCS-02/R1/S01/CH06` | I/O address 'CAB-BPCS-02/R1/S01/CH06' appears 2 times (tag: 200-PT-021, 200-TT-017) | FOC-SPC-IC-001 §6.2.1 | io_list row 51; io_list row 52 |
| 24 | major | IC-005 | `300-TT-005` | 300-TT-005 has a signal type in the instrument index but no channel in the I/O list | FOC-SPC-IC-001 §3.4.1 | instrument_index row 65 |
| 25 | major | IC-007 | `200-PT-011` | Range mismatch: urv: instrument '100' vs io_point '60' | FOC-SPC-IC-001 §3.4.2 | instrument_index row 30 |
| 26 | major | IC-008 | `100-PT-003` | Unit mismatch: eu: instrument 'barg' vs io_point 'bara' | FOC-SPC-IC-001 §4.3.2 | instrument_index row 8 |
| 27 | major | IC-010 | `200-TT-015` | H setpoint 165 degC is outside the instrument range 0–150 degC | FOC-SPC-IC-001 §8.1.2 | alarm_list row 25 |
| 28 | major | IC-010 | `300-VT-031` | HH setpoint 160 µm is outside the instrument range 0–150 µm | FOC-SPC-IC-001 §8.1.2 | alarm_list row 52 |
| 29 | major | IC-010 | `600-AT-010` | H setpoint 60 ppm is outside the instrument range 0–50 ppm | FOC-SPC-IC-001 §8.1.2 | alarm_list row 95 |
| 30 | major | IC-011 | `200-PT-021` | HH setpoint 20 is not above H setpoint 21 (trip setpoint inside the alarm band) | FOC-SPC-IC-001 §8.1.3 | alarm_list row 27; alarm_list row 28 |
| 31 | major | IC-013 | `200-TT-016` | signal 'RTD Pt100' is not compatible with card_type 'AI-HART-IS' (allowed: 4-20mA, 4-20mA HART) | FOC-SPC-IC-001 §6.2.5 | io_list row 50 |
| 32 | major | IC-019 | `CAB-BPCS-02` | 3 spare of 24 installed channels (12%); minimum is 20% | FOC-SPC-IC-001 §6.2.3 |  |
| 33 | major | IC-028 | `300-TT-021` | eu 'barg' is not compatible with instrument_type 'TT' (allowed: degC) | FOC-SPC-IC-001 §4.3.1 | instrument_index row 72 |
| 34 | major | IC-029 | `300-TT-021` | Alarm unit mismatch: eu: alarm 'degC' vs instrument 'barg' | FOC-SPC-IC-001 §3.4.2 | alarm_list row 47 |
| 35 | major | IC-034 | `800-GD-101B` | H setpoint 60 %LEL differs from the standard 50 %LEL for GD | FOC-SPC-IC-001 §8.3.2 | alarm_list row 108 |
| 36 | major | IC-037 | `500-FT-010B` | Meter run B ultrasonic flowmeter: companion instrument(s) 500-TT-010B not in the instrument index | FOC-SPC-IC-001 §5.5.1 | instrument_index row 124 |
| 37 | minor | IC-001 | `100-PT020` | tag '100-PT020' does not match the convention \d{3}-[A-Z]{1,4}-\d{3}[A-Z]? | FOC-SPC-IC-001 §4.2.1 | instrument_index row 19 |
| 38 | minor | IC-021 | `200-FT-018` | Transmitter is not HART-enabled (hart = 0) | FOC-SPC-IC-001 §5.1.3 | instrument_index row 40 |
| 39 | minor | IC-024 | `300-PT-020` | L setpoint 0.5 barg is at 5% of span (0–10); preferred band 10%–90% | FOC-SPC-IC-001 §8.1.2 | alarm_list row 46 |

## Findings per rule

| Rule | Title | Findings |
|---|---|---|
| IC-001 | Tag follows the project tag naming convention | 1 |
| IC-002 | Tag is unique in the instrument index | 1 |
| IC-003 | I/O address is unique | 1 |
| IC-004 | Tag is allocated to only one I/O channel | 0 |
| IC-005 | Every wired instrument has an I/O allocation | 1 |
| IC-006 | Every I/O channel tag exists in the instrument index | 0 |
| IC-007 | Range agrees between instrument index and I/O list | 1 |
| IC-008 | Engineering unit agrees between instrument index and I/O list | 1 |
| IC-009 | Every alarm-list tag exists in the instrument index | 0 |
| IC-010 | Alarm and trip setpoints lie within the instrument range | 3 |
| IC-011 | Alarm hierarchy is ordered LL < L < H < HH | 1 |
| IC-012 | SIF trip setpoint agrees across SIF register, C&E and alarm list | 2 |
| IC-013 | Field signal type is compatible with the I/O card type | 1 |
| IC-014 | Instrument is wired to the system it belongs to (BPCS / SIS segregation) | 2 |
| IC-015 | Intrinsically safe devices are on IS channels | 1 |
| IC-016 | Every C&E cause tag exists in the instrument index | 0 |
| IC-017 | Every C&E effect tag exists in the instrument index | 1 |
| IC-018 | Every SIF initiator and final element exists in the instrument index | 0 |
| IC-019 | Installed spare I/O per cabinet meets the minimum | 1 |
| IC-020 | SIF initiator voting meets the minimum for its SIL | 1 |
| IC-021 | Transmitters use 4-20 mA with HART | 1 |
| IC-022 | Ex certification covers the area classification | 2 |
| IC-023 | Every SIF initiator appears as a cause in the C&E | 0 |
| IC-024 | Alarm and trip setpoints sit within the preferred part of the span | 1 |
| IC-025 | Redundant devices are allocated to different I/O cards | 2 |
| IC-026 | Every C&E row of a SIF initiator actions every final element of that SIF | 1 |
| IC-027 | Initiator voting agrees between SIF register and C&E | 2 |
| IC-028 | Engineering unit suits the measured variable | 1 |
| IC-029 | Alarm-list unit agrees with the instrument index | 1 |
| IC-030 | Every sequence permissive / action tag exists in the instrument index | 1 |
| IC-031 | Sequence step timers respect the limits for their step type | 2 |
| IC-032 | C&E action drives each final element to its stated safe position | 2 |
| IC-033 | Every SIF final element has a safe position in the instrument index | 0 |
| IC-034 | Detector alarm setpoints follow the company standard for their type and unit | 1 |
| IC-035 | Executive F&G actions require confirmation by at least two detectors | 1 |
| IC-036 | A voting group contains at least N initiators for its MooN voting | 1 |
| IC-037 | Every measurement has its required companion instruments (e.g. fiscal meter run P and T) | 1 |

## Generation record

- Run ID: `20261010T152443Z-4de462`  (started 2026-10-10T15:24:43+00:00)
- Engine: iccopilot 0.1.0; rule pack ic-core 0.4.0 (sha 3eb4efc271687c38)
- Data snapshot: `0b4feb1c32b70824`
- Source documents:
  - GPF-IC-LST-003 Rev A - `data/GPF-IC-LST-003_Alarm_Trip_List.xlsx` (sha256 bb2592fefa3f)
  - GPF-SS-LST-002 Rev A - `data/GPF-SS-LST-002_BMS_Sequence.xlsx` (sha256 5a9d55c312c6)
  - GPF-SS-CE-001 Rev A - `data/GPF-SS-CE-001_ESD_Cause_and_Effect.xlsx` (sha256 7ff326afe15e)
  - GPF-SS-CE-002 Rev A - `data/GPF-SS-CE-002_FG_Cause_and_Effect.xlsx` (sha256 842a889b01ef)
  - GPF-IC-LST-001 Rev B - `data/GPF-IC-LST-001_Instrument_Index.xlsx` (sha256 bc57bf467b93)
  - GPF-IC-LST-002 Rev B - `data/GPF-IC-LST-002_IO_List.xlsx` (sha256 e32ea1d59a13)
  - GPF-SS-LST-001 Rev A - `data/GPF-SS-LST-001_SIF_Register.xlsx` (sha256 b8840d879aca)
- Generated by deterministic rules; no language model involved in producing these findings.
- Review: each finding to be accepted or rejected by a named engineer.
