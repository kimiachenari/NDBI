# 🌍 **Landsat 8 NDBI Analysis** 🌍

This project uses **Landsat 8** imagery from 2019 to calculate the **Normalized Difference Built-up Index (NDBI)**, which is used to monitor urban growth and built-up areas. The analysis provides insights into urbanization patterns by assessing the built-up areas over the course of the year. The result is a mean NDBI image for 2019 that highlights areas of urban development.

The tool utilizes **Google Earth Engine** to process satellite imagery, apply the NDBI formula, and export the results in a format suitable for further analysis.

---

## 🚀 **Key Features**

- **Landsat 8 Imagery**: Process Landsat 8 imagery for the year 2019.
- **NDBI Calculation**: Calculate the **Normalized Difference Built-up Index (NDBI)** for detecting built-up areas.
- **Mean NDBI Image**: Generate a mean NDBI image for 2019 that represents the urbanization level across the region of interest.
- **Export Results**: Export the results as a **GeoTIFF** image for further use in GIS applications.

---

## 🧑‍🔬 **Getting Started**

### 1. **Sign up for Google Earth Engine (GEE)**

To run this analysis, you need a **Google Earth Engine (GEE)** account. If you don’t have one, you can sign up [here](https://signup.earthengine.google.com/).

### 2. **Define Your Region of Interest (ROI)**

The script requires a region of interest (ROI) for the analysis. You can define the geometry of your area using **Google Earth Engine** or upload your own region.

```javascript
var geometry = ee.Geometry.Polygon(
  [[[...]]] // Coordinates of your region
);
```

### 3. **Run the Script**

- Open the [Google Earth Engine Code Editor](https://code.earthengine.google.com/).
- Paste the provided code into the editor.
- Hit **Run** to process the **Landsat 8** data for the year 2019 and generate the NDBI mean image.

---

## 🔧 **How It Works**

### 1. **Load Landsat 8 Imagery**

The script loads **Landsat 8 imagery** from the **'LANDSAT/LC08/C02/T1_RT'** collection for the year 2019, based on the defined **startDate** and **endDate**.

```javascript
var landsat8 = ee.ImageCollection('LANDSAT/LC08/C02/T1_RT')
  .filterDate(startDate, endDate)
  .filterBounds(geometry);
```

### 2. **Calculate NDBI**

The **Normalized Difference Built-up Index (NDBI)** is calculated using the **Near-Infrared (B5)** and **Shortwave Infrared (B6)** bands from Landsat 8:

\[
\text{NDBI} = \frac{\text{B6} - \text{B5}}{\text{B6} + \text{B5}}
\]

The result is a new image with the **NDBI** band added.

```javascript
var addNDBI = function(image) {
  var ndbi = image.normalizedDifference(['B6', 'B5']).rename('NDBI');
  return image.addBands(ndbi);
};
```

### 3. **Process the Image Collection**

We apply the **NDBI function** to the **Landsat 8 imagery** for 2019. This results in an image collection where each image has an added **NDBI** band:

```javascript
var ndbiCollection = landsat8.map(addNDBI);
```

### 4. **Generate Mean NDBI Image**

The **NDBI mean** for 2019 is calculated by reducing the image collection to a single image, representing the average NDBI value for each pixel across the year:

```javascript
var ndbiMean = ndbi2019.mean().clip(geometry);
```

### 5. **Visualize the Results**

The **mean NDBI image** is displayed on the map with a color palette that shows areas of low, medium, and high built-up areas:

```javascript
Map.centerObject(geometry, 10);
Map.addLayer(ndbiMean, {min: -0.5, max: 1, palette: ['blue', 'white', 'green']}, 'NDBI 2019');
```

### 6. **Export the Results**

The resulting **mean NDBI image** is exported as a **GeoTIFF** to your **Google Drive** for further analysis in GIS software:

```javascript
Export.image.toDrive({
  image: ndbiMean,
  description: 'NDBI_2019',
  scale: 30,
  region: geometry,
  maxPixels: 1e13
});
```

---

## 📊 **Outputs**

### 1. **Mean NDBI Image for 2019**

The **mean NDBI image** highlights urbanized areas (built-up regions). These areas have higher NDBI values, and you can analyze changes over the year.

### 2. **Exported Data**

- The **mean NDBI image** will be exported as a **GeoTIFF** file to your **Google Drive**.
- You can use this **GeoTIFF** in GIS tools such as **QGIS** or **ArcGIS** for further analysis.

---

## 📈 **Visualization**

The generated **NDBI map** uses a color palette to represent areas with varying levels of urbanization:

- **Blue**: Low urbanization (rural areas, vegetation)
- **Green/White**: Higher levels of built-up areas (urbanized regions)

This provides an intuitive way to visualize urban growth and development across the study area.

---

## 📅 **Future Enhancements**

- **Time-series Analysis**: Expand the analysis to include data from multiple years for monitoring urban growth trends.
- **Integration with Other Indices**: Combine NDBI with other indices such as **NDVI** (Normalized Difference Vegetation Index) or **EVI** (Enhanced Vegetation Index) for a more comprehensive urbanization analysis.
- **Advanced Export Options**: Implement additional export formats, such as **CSV** for pixel-level data.


---

This **README** provides an overview of the script, how to use it, and the outputs you can expect. Let me know if you'd like any further tweaks or explanations!
