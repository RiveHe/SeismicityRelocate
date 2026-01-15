# Surface-wave Cross-correlation Relocation of OTF Earthquakes (Gofar)

•	Wei, M., He, L., & Smith-Konter, B. (2024). A model of the earthquake cycle along the Gofar oceanic transform faults. Seismica.

This repository contains code to **relocate OTF earthquake clusters at Gofar** using **cross-correlation of teleseismic Rayleigh surface waves**. The workflow follows established approaches that use **differential surface-wave arrival times** across a wide azimuthal station distribution to estimate **relative event-to-event location offsets**, then propagates those offsets through **chains of event pairs** to relocate clusters.

Key references for the method include McGuire (2008), Cleveland & Ammon (2015), Wolfson-Schwehr et al. (2014), Howe et al. (2019), Castellanos et al. (2020), Shi et al. (2022).

---

## Method summary

1. **Event selection**
   - Events are queried from the **USGS earthquake catalog** between **1950-01-01** and **2023-11-01**.
   - Relocations are performed primarily for events **after 1990** (with most after **1995**) when global station coverage is generally adequate for this task.

2. **Waveform data**
   - Teleseismic waveform data are downloaded for stations from:
     - **Global Seismic Network (GSN)** (network code `GSN`)
     - **GEOSCOPE** (network code `G`)
   - These networks provide satisfactory azimuthal coverage for surface-wave differential timing.

3. **Preprocessing**
   - Assume **R1 Rayleigh wave group velocity**: **3.75 km/s** (Nishimura & Forsyth, 1988).
   - Raw waveforms are **truncated** with a velocity window of **5 km/s to 3 km/s** around the predicted surface-wave arrival.
   - Apply **zero-phase bandpass filter** between **0.02–0.04 Hz**, then **taper**.

4. **Cross-correlation and differential times**
   - For each event pair, compute **cross-correlation** to obtain **differential arrival times** at stations spanning different azimuths.

5. **Cosine fitting across azimuth**
   - Fit a cosine model to differential times as a function of station azimuth to estimate:
     - relative separation (distance) between events
     - relative azimuth (direction)
     - uncertainties

6. **Cluster relocation via event-pair chaining**
   - Relative offsets from many event pairs are combined (propagated through chains) to collectively relocate events within each cluster (e.g., Cleveland & Ammon, 2015; Shi et al., 2022).

7. **Absolute shift / geologic anchoring (optional)**
   - The final relocated cluster can be shifted/anchored using:
     - accurate **hydroacoustic catalogs**, if available, and/or
     - geological constraints from **high-resolution bathymetry** (e.g., Pan et al., 2002).

---

## Data sources

Catalog and waveform sources used by this project (links included for convenience):

```text
USGS Earthquake Catalog:
https://earthquake.usgs.gov/earthquakes/search

Global Seismic Network (GSN):
https://www.iris.edu/hq/programs/gsn   (network code: GSN)

GEOSCOPE:
http://geoscope.ipgp.fr/networks/detail/G/   (network code: G)
