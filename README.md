# Head & Neck Cancer Records, South-West Nigeria (1990–2020) — LGA level

Records of head and neck cancers from a hospital cancer registry in South-West
Nigeria, spanning 1990–2020. Each row is one recorded case with demographic
fields, primary site, diagnosis, and the **Local Government Area (LGA)** of the
patient's residence.

**File:** `ready_geo_lga.csv` (820 records)

> **Patient-level coordinates have been removed.** This release replaces the
> earlier WGS84 point geometry (~11 m precision) with LGA-level geography, to
> prevent re-identification of patients with rare tumours. See
> [`DATA_AVAILABILITY.md`](DATA_AVAILABILITY.md).

## Quick facts

- **Period:** 1990 – 2020
- **Records:** 820
- **Sex:** Male 478 · Female 342
- **Age category:** Adult 661 · Paediatric 159
- **Top major sites:** Thyroid gland (113), Mandible (111), Oesophagus (106), Cervical lymph nodes (85), Orbit (78), Oral (55)
- **Common diagnoses:** Squamous cell carcinoma (234), Burkitt lymphoma (79), Retinoblastoma (43), Papillary adenocarcinoma (40), Non-Hodgkin lymphoma (38)
- **Flags:** carcinoma 448 · lymphoma 135 · metastatic 40 · cervical node metastases 32
- **Geography:** 35 LGAs across 10 states — Osun 615, Ondo 105, Ekiti 59, Oyo 18, Lagos 10, Kwara 5, FCT 3, Delta 2, Ogun 2, Kogi 1
- **Largest LGAs by case count:** Atakunmosa West 226, Osogbo 177, Ife Central 148, Ondo West 70, Ado-Ekiti 52

## Columns

| Column | Description |
|---|---|
| record_id | Sequential record identifier (renumbered; not linked to any registry record number) |
| Year | Year of record (1990–2020) |
| Years Grouped | 5-year band, e.g. 1990–1994 |
| Decade | First/Second/Third/Fourth decade of the series |
| AgeConvertedTo | Numeric age (midpoint where banded) |
| Age Group | Age band, e.g. 0–9, 10–19 |
| SEX | Male / Female |
| LGA | **Local Government Area** of residence, assigned from the 2022 GRID3 LGA boundaries (774 units) |
| STATE | State containing the LGA (boundary-derived; complete, no blanks) |
| SITE | Anatomical subsite, e.g. ORBIT |
| MAJOR SITE | Grouped site, e.g. ESOPHAGUS |
| DIAGONISIS | Diagnosis (field name kept as in source) |
| ISMETASTATIC | TRUE/FALSE — metastatic disease |
| ISCARCINOMA | TRUE/FALSE — carcinoma |
| ISLYMPHOMA | TRUE/FALSE — lymphoma |
| CERVICAL METS | TRUE/FALSE — cervical node metastases |
| AGE_CATEGORY | Paediatric / Adult |
| STATE_RECORDED | State as recorded in the source extract (blank where the source was blank). Retained for provenance; disagrees with `STATE` in 6 of 820 rows. |

### How LGA was assigned

The original release carried a WGS84 point per record, rounded to 4 decimal
places (~11 m). Each point was assigned to exactly one of the 774 Nigerian LGAs
by a local point-in-polygon against the geoBoundaries/GRID3 ADM2 layer
(boundary year represented: 2022, CC BY 4.0). All 820 points fell inside exactly
one LGA polygon — none was ambiguous and none fell outside the boundary set.
The point itself is **not** retained in any form.

Where the source recorded a state, the LGA-derived state agrees with it in 588
of 594 rows (99%). The derived state additionally resolves the ~47% of rows whose
`STATE` was blank in the source.

## Data notes

- The diagnosis field header `DIAGONISIS` is a typo preserved from the source extract.
- `SN` from the earlier release has been replaced by a fresh sequential `record_id` so rows cannot be matched back to the source extract.
- Rows lacking usable coordinates were excluded from geocoding by the source pipeline and are not present.
- No patient names, addresses, hospital numbers, record numbers, free-text fields, or dates of birth appear in this file.

## Quick start

```python
import pandas as pd

df = pd.read_csv("ready_geo_lga.csv")
df.groupby("LGA").size().sort_values(ascending=False).head()
```

To join LGA polygons for mapping, use any Nigerian LGA boundary layer
(774 units) keyed on LGA name, e.g. GRID3 / geoBoundaries ADM2:

```python
import geopandas as gpd

lga = gpd.read_file("geoBoundaries-NGA-ADM2.geojson")      # shapeName == LGA
df = pd.read_csv("ready_geo_lga.csv")
gdf = lga.merge(df.groupby(["LGA", "STATE"]).size().rename("cases"),
                left_on="shapeName", right_index=True)
gdf.plot(column="cases", legend=True)
```

## Privacy & de-identification

The original registry extract contained free-text patient addresses and hospital
record numbers; those were removed before the first public release. The first
release still carried point coordinates at ~11 m precision, which is too precise
for records that include rare paediatric tumours. Those coordinates were removed
on **2026-10-23** and replaced with LGA-level geography. No residual location
data finer than LGA is published or retained in this repository's history.

## License

[CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) — see `LICENSE`.

Boundary data used to derive LGA is © GRID3 / geoBoundaries, CC BY 4.0.

## Citation

If you use this dataset, please cite:

> Ameye, S. (2026). *Head and neck cancer records, South-West Nigeria (1990–2020), LGA level* [Data set]. GitHub. https://github.com/ameye/hnc-nigeria-geospatial
