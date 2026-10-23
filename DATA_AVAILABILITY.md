# Data availability

## What is provided

`ready_geo_lga.csv` — 820 head and neck cancer registry records from a hospital
cancer registry in South-West Nigeria, 1990–2020, with each record reduced to the
**Local Government Area (LGA)** of the patient's residence, plus the containing
state.

The file contains no coordinates, no dates of birth, no addresses, no hospital
or record numbers, and no free-text fields. Every remaining field is a coded
clinical, demographic, temporal, or geographic (LGA-level) value.

## What was removed, and when

**Date of removal: 2026-10-23.**

The earlier release of this dataset (`ready_geo.csv`) included, for every one of
the 820 records, a WGS84 latitude and longitude rounded to four decimal places
and a corresponding WKT point geometry — 3280 coordinate values in total, at
approximately 11 m horizontal resolution.

At that resolution a point identifies a building or a small cluster of buildings.
Because this registry includes rare tumours — 159 of the 820 records are
paediatric, and diagnoses such as retinoblastoma (43), Burkitt lymphoma (79) and
other childhood tumours occur only a few times per site and period — a precise
point on such a record is close to naming an individual child. Point-level
geography was therefore removed as re-identifiable health data, and is not
published in this repository in any branch, tag, release asset, or commit.

## Granularity now published

LGA — 774 Local Government Areas nationally; 35 LGAs are represented in this
dataset, across 10 states. This matches the LGA granularity used in the author's
other Nigerian geospatial work.

LGA was assigned from the removed coordinates by local point-in-polygon against
the geoBoundaries ADM2 layer for Nigeria (boundary year 2022, source GRID3,
CC BY 4.0, 774 units). All 820 points fell within exactly one LGA polygon; no
point was ambiguous or outside the boundary set. Only the resulting LGA name is
retained — the point used to derive it was discarded.

## Effect on the analysis

The spatial analysis is preserved at LGA level. All clinical, demographic and
temporal fields are unchanged and are carried over row for row:

- year of record, 5-year band, and decade of the series
- age (numeric and banded), sex, age category (paediatric / adult)
- anatomical subsite and grouped major site
- diagnosis
- metastatic, carcinoma, lymphoma and cervical-node-metastasis flags

Distribution checks confirm that every clinical field has exactly the same value
counts as the pre-removal release (820 records; Male 478 / Female 342; Adult 661 /
Paediatric 159; identical site, diagnosis and flag frequencies). Analyses that
aggregate by LGA, state, decade or site reproduce unchanged; analyses that
required sub-LGA distance, cluster detection or point-density estimation can no
longer be run from the public data.

## Requesting finer access

Patient-level coordinates are not available for public release. Any request for
access to finer-grained data should be directed to the data custodian at the
originating institution, subject to ethics approval and a data use agreement, and
should not be routed through this repository.

## Residual considerations

- Some LGAs contribute a single paediatric record. LGA is a standard disclosure
  level for cancer registry data, but a cross-tabulation of LGA × year × a very
  rare diagnosis may still describe a very small number of children. Users should
  avoid publishing cells of size one or two.
- `AgeConvertedTo` carries fractional ages for some infant records. It is an age at
  diagnosis, not a date of birth.

## History

The repository history was rewritten on 2026-10-23 so that the coordinate-bearing
file does not exist in any commit. The current history consists of a single root
commit containing only the files listed above.
