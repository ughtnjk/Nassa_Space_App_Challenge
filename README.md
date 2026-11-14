NISAR Radar Data Exploration, Clustering, and Anomaly Detection

Project description

This Google Colab notebook reads a NASA-ISRO Synthetic Aperture Radar
(NISAR) GCOV HDF5 product from Google Drive and explores its HHHH and
HVHV radar measurements. It creates radar visualizations, derives
pixel-level features, groups radar signatures using MiniBatch K-Means,
and identifies candidate anomalous pixels and spatial regions using
Isolation Forest.
Features
- Authenticates with Google Colab and accesses a file in Google Drive
  through the Google Drive API.
- Inspects HDF5 product structure, metadata, dataset shapes, data
  types, and attributes.
- Reads the HHHH and HVHV datasets under
  science/LSAR/GCOV/grids/frequencyA/.
- Visualizes the original measurements, backscatter values in decibels
  (dB), and a false-colour composite.
- Extracts five features per valid sampled pixel: HHHH dB, HVHV dB,
  HHHH--HVHV ratio in dB, and local standard deviations for both
  channels.
- Applies standardization and MiniBatch K-Means clustering with four
  clusters.
- Uses Isolation Forest to flag the highest-scoring 2% of valid pixels
  as candidate anomalies.
- Groups neighbouring candidate anomaly pixels into regions and
  summarizes their measurements.
Requirements
The notebook is designed for Google Colab and uses: -
google-api-python-client - h5py - numpy - pandas -
matplotlib - scikit-learn - scipy

The notebook installs the required Python packages in its setup cells.
Input data
A NISAR GCOV HDF5 file accessible through the Google account used in
Colab. The notebook currently uses a Google Drive file ID stored in the
FILE_ID variable. Replace that ID with the ID of the intended file if
needed. The expected datasets are:
- science/LSAR/GCOV/grids/frequencyA/HHHH
- science/LSAR/GCOV/grids/frequencyA/HVHV
How to run
1. Open the notebook in Google Colab.
2. Ensure the NISAR GCOV HDF5 file is accessible to the Google account
   used in the notebook.
3. Update FILE_ID if you want to process a different Drive file.
4. Run the cells in order, including the Google authentication prompts.
5. Review the metadata output, radar visualizations, cluster map,
   anomaly maps, and candidate-region table.
Processing notes
- STRIDE = 20 is used to subsample the raster for visualization and
  analysis, reducing memory and processing requirements. The analysis
  therefore operates on sampled pixels rather than every source pixel.
- Positive finite backscatter values are converted to dB using
  10 * log10(value).
- K-Means cluster IDs are arbitrary group labels; they do not by
  themselves identify land-cover classes or physical surface types.
- Isolation Forest scores indicate statistical unusualness relative to
  the sampled feature distribution. Flagged pixels and regions are
  candidates for further inspection, not confirmed defects or events.
- The 2% anomaly cutoff is controlled by the percentile threshold in
  the notebook. The minimum retained region size is five sampled
  pixels.
Outputs
The notebook displays: - HHHH and HVHV radar images. - A false-colour
HHHH/HVHV composite. - Metadata and dataset information. - A
four-cluster radar-signature map and cluster counts. - An anomaly-score
map and anomaly overlay. - A table and map of retained candidate anomaly
regions.
Limitations
The notebook does not include labelled ground truth, supervised
classification, or validation of detected anomalies against independent
reference data. Results depend on the selected input product, sampling
stride, feature design, clustering settings, and anomaly threshold.
Interpret candidate regions as exploratory results until they are
independently verified.
Reproducibility
The clustering and Isolation Forest models use random_state=42. The
notebook also uses a fixed sampling stride and fixed analysis
parameters, which can be adjusted for other datasets or experiments.
