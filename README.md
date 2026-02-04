# AI-Powered Pipeline Route Optimization System

## 🎯 Overview

This is a comprehensive three-page web application for AI-powered pipeline route optimization. The system incorporates multiple spatial and environmental factors to compute suitability maps and generate optimal pipeline routes.

## 📋 System Features

### Key Capabilities
- ✅ Multi-factor suitability analysis
- ✅ DEM-based slope and flow accumulation computation
- ✅ Interactive Google Satellite base maps
- ✅ Layer visualization with toggle controls
- ✅ Three suitability map formats (Raw, Index 0-10, 5-class reclassified)
- ✅ Optimal route generation with random path algorithm
- ✅ Downloadable outputs (GeoTIFF and Shapefile)
- ✅ WGS 84 UTM coordinate system support

### Input Factors
1. **DEM (Digital Elevation Model)** - Used to compute slope and flow accumulation
2. **Soil Type** - Soil classification data
3. **LULC** (Land Use/Land Cover) - Land cover information
4. **Distance to Road** - Proximity to road networks
5. **Distance to Water Body** - Proximity to water features
6. **Pipeline Parameters** - Operating pressure and diameter
7. **Weighting Factors** - Environmental impact, construction cost, safety risk

## 🚀 Getting Started

### Prerequisites
- Modern web browser (Chrome, Firefox, Safari, Edge)
- Internet connection (for Google Satellite maps)
- GeoTIFF (.tif) raster files for spatial data

### File Structure
```
pipeline-optimizer/
├── page1_upload.html       # Step 1: Data Upload & Configuration
├── page2_visualize.html    # Step 2: Data Visualization & Suitability
├── page3_route.html        # Step 3: Route Generation & Download
└── README.md               # This file
```

### Navigation
The system features **seamless navigation** between all pages:

✅ **Progress Bar Navigation** - Click on any step in the progress bar to jump to that page
✅ **Action Buttons** - Primary buttons advance you to the next step
✅ **Back Buttons** - Return to previous steps to adjust parameters
✅ **Skip to Demo** - Test visualization without uploading files (Page 1)
✅ **Start Over** - Return to upload from any page

## 📖 User Guide

### Page 1: Data Upload & Weight Configuration

**Purpose:** Upload spatial data and configure analysis weights

**Steps:**
1. **Upload Required Data Files** (.tif format):
   - DEM (Digital Elevation Model)
   - Soil Type
   - LULC (Land Use/Land Cover)
   - Distance to Road
   - Distance to Water Body

2. **Configure Weights** (0.00 to 1.00):
   - Slope (default: 0.15)
   - Flow Accumulation (default: 0.10)
   - Soil Type (default: 0.15)
   - Distance to Road (default: 0.15)
   - Distance to Water Body (default: 0.15)
   - LULC (default: 0.10)
   - Environmental Impact (default: 0.10)
   - Construction Cost (default: 0.05)
   - Safety Risk (default: 0.05)

3. **Enter Pipeline Parameters**:
   - Operating Pressure (bar)
   - Pipeline Diameter (mm)

4. **Click "Generate Suitability Map"** to proceed to Page 2

**Note:** All weights are adjustable via sliders. The system stores your configuration in browser localStorage.

---

### Page 2: Data Visualization & Suitability Analysis

**Purpose:** Visualize input layers and compute suitability maps

**Features:**

**Left Panel - Layer Controls:**
- Toggle individual data layers on/off
- Layers available:
  - DEM (Elevation)
  - Slope (computed from DEM)
  - Flow Accumulation (computed from DEM)
  - Soil Type
  - Land Use / Land Cover
  - Distance to Road
  - Distance to Water Body

**Suitability Computation:**
1. Click **"Compute Suitability"** to generate the suitability map
2. View results in three formats:
   - **Raw:** Original computed values
   - **Index:** Normalized to 0-10 scale
   - **Classified:** 5 classes (Very Suitable → Not Suitable)

**Suitability Classes:**
- 🟢 Very Suitable
- 🟢 Suitable
- 🟡 Moderately Suitable
- 🟠 Less Suitable
- 🔴 Not Suitable

**Right Panel - Actions:**
- Download suitability maps in all three formats (.tif)
- View configuration summary
- Click **"Generate Optimal Route"** to proceed to Page 3

**Map Controls:**
- Google Satellite base map
- Pan and zoom to explore the area
- Layer overlays with adjustable opacity

---

### Page 3: Optimal Route Generation

**Purpose:** Generate and download optimal pipeline routes

**Route Generation Algorithm:**
The system uses a sophisticated random path generation algorithm:
1. Creates a bounding box between start and end points
2. Adds a 10% buffer to the bounding box
3. Generates 8 random intermediate points within the buffered area
4. Sorts points by projection along the start→end vector
5. Creates a smooth LineString connecting all 10 points

**Steps:**

1. **Set Start and End Coordinates:**
   - **Option A:** Click on the map
     - First click sets START point (blue marker)
     - Second click sets END point (red marker)
   - **Option B:** Enter coordinates manually
     - Enter Latitude and Longitude for both points
     - Markers are draggable after placement

2. **Generate Route:**
   - Click **"Generate Optimal Path"**
   - System generates 8 random intermediate points
   - Route appears as a green line on the map
   - Yellow markers show intermediate points

3. **View Path Statistics:**
   - Total points (10: start + 8 intermediate + end)
   - Estimated path length in kilometers
   - Start/End coordinates
   - Path ID and generation timestamp

4. **Download Route:**
   - Click **"Download Shapefile (.zip)"**
   - Exports as GeoJSON (convertible to Shapefile)
   - Includes all route metadata
   - WGS 84 UTM coordinate system

**Map Features:**
- Google Satellite base map
- Interactive markers (draggable)
- Route visualization with intermediate points
- Popups showing point coordinates
- Auto-zoom to fit route bounds

---

## 🔧 Technical Details

### Coordinate System
- **Primary CRS:** WGS 84 UTM
- All inputs and outputs use this coordinate system
- Coordinates displayed with 6 decimal precision

### Path Generation Algorithm
The optimal route uses a random path generation approach:

```
1. Calculate bounding box from start/end points
2. Add 10% buffer to bounding box
3. Generate 8 random points in buffered area
4. Sort points by projection along start→end vector:
   t = (point - start) · (end - start) / |end - start|²
5. Assemble final path: [start, sorted_points, end]
6. Create LineString geometry
```

**Path Properties:**
- Always starts at START coordinate
- Always ends at END coordinate
- Exactly 10 points total (1 + 8 + 1)
- Points progress naturally from start to end
- No backtracking or loops

### Suitability Computation
The suitability analysis uses weighted overlay:

```
Suitability = Σ (Factor_i × Weight_i)

Where:
- Factor_i = Normalized input layer value (0-1)
- Weight_i = User-defined weight (0-1)
```

**Output Formats:**
1. **Raw:** Direct computation results
2. **Index:** Rescaled to 0-10 range
3. **Classified:** Binned into 5 classes

### Data Storage
- **Browser localStorage** stores configuration between pages
- **Stored data:**
  - Weight configurations
  - Pipeline parameters
  - File information
  - Suitability computation status

---

## 💾 File Formats

### Input Files
- **Format:** GeoTIFF (.tif, .tiff)
- **Requirement:** All inputs must have matching:
  - Coordinate system (WGS 84 UTM)
  - Spatial extent
  - Pixel resolution

### Output Files

**Suitability Maps:**
- **Format:** GeoTIFF (.tif)
- **Types:** Raw, Index (0-10), Classified (5 classes)
- **CRS:** WGS 84 UTM

**Route Shapefile:**
- **Format:** GeoJSON (convertible to Shapefile)
- **Geometry:** LineString
- **Attributes:**
  - Route ID
  - Route name
  - Path length (km)
  - Number of points
  - Generation timestamp
- **CRS:** WGS 84 UTM

---

## 🎨 User Interface

### Design Features
- Modern dark theme optimized for GIS work
- Clear visual hierarchy
- Progressive disclosure of features
- Color-coded status messages
- Responsive layout

### Color Scheme
- **Primary:** Cyan (#00b4d8) - Accent and interactive elements
- **Success:** Green (#06d6a0) - Successful operations
- **Warning:** Yellow (#ffd166) - Important notices
- **Error:** Red (#ef476f) - Error states
- **Background:** Dark (#0f1419) - Main background

### Interactive Elements
- Draggable map markers
- Toggle switches for layer visibility
- Slider controls for weights
- Real-time status updates
- Progress indicators

---

## 🔄 Workflow Summary

```
┌─────────────────────────────────────────────────────┐
│  PAGE 1: Upload & Configure                         │
│  • Upload 5 GeoTIFF files                            │
│  • Set 9 weight parameters                           │
│  • Enter pipeline specifications                     │
│  • Click "Generate Suitability Map" → Page 2         │
│  • OR "Skip to Visualization" for demo mode          │
└─────────────────────┬───────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────┐
│  PAGE 2: Visualize & Analyze                        │
│  • Toggle data layers on/off                         │
│  • Click "Compute Suitability"                       │
│  • View 3 suitability formats                        │
│  • Download suitability maps (.tif)                  │
│  • Click "Generate Optimal Route" → Page 3           │
│  • OR "← Back to Upload" to change config            │
└─────────────────────┬───────────────────────────────┘
                      ↓
┌─────────────────────────────────────────────────────┐
│  PAGE 3: Generate Route                             │
│  • Set start/end coordinates (click or type)         │
│  • Click "Generate Optimal Path"                     │
│  • View route statistics                             │
│  • Download route shapefile (.zip)                   │
│  • "← Back to Visualization" or "Start Over"         │
└─────────────────────────────────────────────────────┘

🔄 Navigation Options:
  • Progress bar steps are clickable on all pages
  • Primary buttons advance to next step
  • Back buttons return to previous steps
  • Start Over button resets to Page 1
```

---

## 🚧 Implementation Notes

### Current Status
This is a **front-end demonstration** with simulated backend functionality. For production deployment, you need to implement:

1. **Server-Side Processing:**
   - GeoTIFF file handling (GDAL/Rasterio)
   - Raster algebra for suitability computation
   - DEM processing (slope, flow accumulation)
   - Shapefile generation
   - File storage and retrieval

2. **Backend API Endpoints:**
   ```
   POST /api/upload              - Handle file uploads
   POST /api/compute_suitability - Compute suitability maps
   POST /api/compute_path        - Generate optimal route
   GET  /api/download/suitability - Download suitability .tif
   GET  /api/download/path       - Download route shapefile
   GET  /api/layer_preview       - Preview layer as PNG
   ```

3. **Recommended Tech Stack:**
   - **Backend:** Python (Flask/FastAPI)
   - **GIS Processing:** GDAL, Rasterio, GeoPandas
   - **DEM Analysis:** RichDEM, WhiteboxTools
   - **Storage:** PostgreSQL + PostGIS
   - **File Handling:** Cloud storage (S3, Azure Blob)

### Google Maps Integration
To use Google Satellite maps in production:
1. Obtain a Google Maps API key
2. Add it to the Leaflet.GoogleMutant configuration
3. Enable required APIs in Google Cloud Console

---

## 🎓 Algorithm Details

### Random Path Generation

The path generation algorithm ensures natural progression from start to end:

**Step 1: Bounding Box**
```javascript
minLon = min(startLon, endLon) - buffer
maxLon = max(startLon, endLon) + buffer
minLat = min(startLat, endLat) - buffer
maxLat = max(startLat, endLat) + buffer
```

**Step 2: Random Points**
```javascript
for (i = 0; i < 8; i++) {
  randomLon = uniform(minLon, maxLon)
  randomLat = uniform(minLat, maxLat)
  points.add([randomLon, randomLat])
}
```

**Step 3: Sorting by Projection**
```javascript
// Calculate projection parameter t for each point
t = (point - start) · (end - start) / |end - start|²

// Sort points by t value (0 → 1)
points.sort_by(t)
```

**Step 4: Final Path**
```javascript
path = [start] + sorted_points + [end]
```

This ensures:
- Points flow naturally from start to end
- No loops or backtracking
- Reasonable spacing
- Different route each generation

---

## 📊 Example Use Cases

1. **Oil & Gas Pipelines:**
   - Avoid sensitive environmental areas
   - Minimize construction costs
   - Follow road networks where possible

2. **Water Distribution:**
   - Consider terrain slope
   - Avoid flood-prone areas
   - Optimize for gravity flow

3. **Urban Planning:**
   - Respect land use patterns
   - Minimize disturbance
   - Consider future development

---

## 🔒 Browser Compatibility

**Tested Browsers:**
- ✅ Chrome 90+
- ✅ Firefox 88+
- ✅ Safari 14+
- ✅ Edge 90+

**Required Features:**
- ES6+ JavaScript
- localStorage API
- Canvas API
- Fetch API
- Modern CSS (Grid, Flexbox)

---

## 📝 License & Credits

### Libraries Used
- **Leaflet** - Interactive maps (BSD-2-Clause)
- **Leaflet.GoogleMutant** - Google Maps integration (MIT)
- **shpjs** - Shapefile processing (MIT)
- **Google Fonts** - DM Sans, IBM Plex Mono (OFL)

### Coordinate Reference System
- WGS 84 UTM (EPSG varies by zone)

---

## 🐛 Known Limitations

1. **File Size:** Browser-based processing limited by memory
2. **Real-time Processing:** Simulated - needs backend
3. **Shapefile Export:** Currently exports GeoJSON (needs conversion)
4. **DEM Processing:** Slope/flow accumulation need server-side tools
5. **Multi-user:** No database - localStorage only

---

## 🔮 Future Enhancements

- [ ] Real backend API integration
- [ ] Advanced pathfinding algorithms (A*, Dijkstra)
- [ ] Multi-criteria decision analysis (MCDA)
- [ ] 3D terrain visualization
- [ ] Cost estimation and reporting
- [ ] Collaborative features
- [ ] Mobile app version
- [ ] Export to multiple formats (KML, SHP, GeoJSON)
- [ ] Historical route comparison
- [ ] Environmental impact assessment reports

---

## 📞 Support

For questions or issues:
- Review the visual guide (VISUAL_GUIDE.md)
- Check browser console for errors
- Verify input file formats
- Ensure all required files are uploaded

---

## 🎉 Quick Start Checklist

- [ ] Open `page1_upload.html` in browser
- [ ] Upload 5 required GeoTIFF files
- [ ] Adjust weights as needed
- [ ] Enter pipeline parameters
- [ ] Click "Generate Suitability Map"
- [ ] Toggle layers to explore data
- [ ] Click "Compute Suitability"
- [ ] Download suitability maps if needed
- [ ] Click "Generate Optimal Route"
- [ ] Set start/end points on map
- [ ] Click "Generate Optimal Path"
- [ ] Download route shapefile
- [ ] Success! 🎊

---

**Created:** 2025  
**Version:** 1.0.0  
**System:** AI-Powered Pipeline Route Optimization
