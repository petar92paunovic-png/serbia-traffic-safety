# Traffic Safety Analytics Pipeline (Serbia 2015–2026)
### Data Journalism Investigation | Open Data Portals Integration

This repository contains the data acquisition, normalization, and processing pipeline for a comprehensive data journalism investigation into traffic fatalities and structural accident causes in the Republic of Serbia across an 11-year timeline.

---

## 1. Editorial Context & Hypothesis
In Serbia, an average of approximately 500 people lose their lives in traffic accidents annually. This project programmatically aggregates raw law enforcement records to test hypotheses surrounding:
* **Temporal Peaks:** Identifying seasonal, weekly, and hourly spikes.
* **Geographical Hotspots:** Mapping "black spots" across regional Police Directorates.
* **Structural Correlates:** Extracting accident mechanics and environmental dynamics.

---

## 2. Technical Methodology & Architecture Shift

Initially, the project was conceptualized around manual asset downloads. However, an inspection of the National Open Data Portal (`data.gov.rs`) revealed a unified oEmbed script referencing a singular dataset ID: `5ccbf4597de2722e5424fe97`. This allowed for an architectural simplification, though it brought unexpected challenges.

### The "Restart Kernel & Run All" Collision
Upon executing a hard reset and linear compilation of the workflow, the pipeline experienced a fatal execution error during historical sorting: 
`TypeError: '<' not supported between instances of 'str' and 'float'`

### Post-Mortem Diagnostics & Root Cause Analysis
Running an isolated structural audit on the individual stream objects exposed two severe administrative anomalies:
1. **Headerless Data Layouts:** The source spreadsheets do not contain structural headers. Line 1 of every annual file initializes immediately with raw telemetry records.
2. **Implicit DataFrame Generation:** Pandas automatically promoted the first operational data row of each file to act as the column index. Because tracking IDs change with every event, concatenating these files caused a horizontal explosion of 51 mismatched columns.

---

## 3. The Final Optimized Strategy
To achieve a production-grade, reproducible pipeline, I condensed the architecture into two execution cells:

1. **Cell 1 (Positional Ingestion Loop):** I forced Pandas to read all incoming binary streams with `header=None`. This overrides the promotion of data values to headers, generating a unified index layout of exactly 9 positional variables.
2. **Cell 2 (Direct English Variable Localization):** To prepare the dataset for international publication, I programmatically mapped the 9 positional columns (0–8) to localized English attributes (`accident_id`, `police_directorate`, `timestamp`, etc.).

---

## 4. Methodological Decisions & Research Integrity

**Decision: Truncating the Timeline (Excluding 2015)**
During the data integrity audit, I identified a critical systematic anomaly in the 2015 cohort. Total recorded incidents hovered unrealistically low (fewer than 200 cases per month) between January and August, followed by an artificial exponential surge toward December. 

This indicated that the Ministry of Internal Affairs (MUP) either implemented the digital logging pipeline mid-year or uploaded a fragmented historical back-log. To safeguard the research from structural bias and prevent generating false baseline drops in longitudinal charts, I made the strategic decision to **exclude the year 2015 entirely**. Consequently, all analytical operations strictly utilize the standardized, full-calendar decade spanning 2016 through 2025.

---

## 5. Skills, Growth & Future Work
* **Technical:** I moved from manual data handling to automated pipeline creation, mastering the parsing of "ragged" government datasets.
* **Journalistic:** I developed the ability to perform "post-mortem" diagnostics on government data to identify systematic biases, recognizing that data is often a reflection of administrative workflows rather than objective reality.
* **Future Work:** My next goal is to incorporate geospatial mapping to identify specific "accident black spots" by municipality and correlate these with real-time environmental data.

## 🛠 Tools
- **Data Ingestion:** Python (Pandas)
- **Data Processing:** Positional column mapping and stream manipulation
- **Visualization:** Datawrapper, Pexels, Unsplash, for gif ezgif.com
- **Repository Management:** GitHub Pages
