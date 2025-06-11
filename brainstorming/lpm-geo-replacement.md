# LPM Geospatial System Replacement Options

*Note: Cost reduction and minimizing licensing fees is a priority. The current ArcGIS implementation is not considered for replacement.*

Below are a few potential replacement options along with high-level POC ideas using C# / .NET:

1. **SQL Server Spatial Extensions**
   - **Option:** Leverage SQL Server’s spatial data types (geometry/geography) to store morphology layers.
   - **POC Idea:** Create a .NET service that takes antenna parameters, computes the pie wedge footprint, and queries SQL Server to determine the morphology with the largest area underneath the footprint.
   
   **Technical Implementation Details:**
   - **Spatial Data Storage:**
     - Use `geography` type for real-world coordinates (supports geodetic data)
     - `STGeomFromText()` for creating geometries from WKT
     - Native spatial indexing support for query optimization
   
   - **Geometric Operations:**
     - `STIntersection()` for finding overlapping areas
     - `STArea()` for calculating intersection sizes
     - `STBuffer()` and `STStartPoint()` for pie wedge creation
     - Built-in coordinate system transformations
   
   - **Performance Considerations:**
     - Spatial indexes on morphology polygons
     - Batch operations using table-valued parameters
     - Partitioning for large datasets

2. **NetTopologySuite Integration**
   - **Option:** Use NetTopologySuite for geometric operations and spatial analysis entirely within .NET.
   - **POC Idea:** Develop a .NET application to simulate the calculation of antenna footprints and evaluate overlapping morphological areas using NetTopologySuite’s geometry processing utilities.
   
   **Technical Implementation Details:**
   - **Antenna Footprint Creation:**
     - Use `GeometricShapeFactory` to create the pie wedge shape
     - `CreateArc()` method for creating the curved portion
     - `CreatePolygon()` to complete the wedge shape
   
   - **Morphology Analysis:**
     - Store morphology polygons using `Polygon` or `MultiPolygon` geometries
     - Use `Geometry.Intersection()` to find overlapping areas
     - `Geometry.Area` to calculate the size of intersecting regions
     - `PreparedGeometry` class for optimized spatial operations when processing multiple antennas
   
   - **Data Import/Export:**
     - `WKTReader` and `WKTWriter` for importing/exporting Well-Known Text format
     - `GeoJsonReader` and `GeoJsonWriter` for GeoJSON support
     - Built-in coordinate system transformations via `CoordinateSystemFactory`

   - **Performance Optimization:**
     - Implement spatial indexing using `STRtree` for quick geometry lookups
     - Use `PreparedGeometryFactory` to optimize repeated operations
     - Batch process antenna calculations using parallel operations
  
3. **Cloud-based Spatial Processing with Azure Maps**
   - **Option:** Utilize Azure Maps spatial services to manage geospatial data at scale, with an eye toward low licensing costs.
   - **POC Idea:** Implement a .NET microservice that consumes Azure Maps REST APIs to process geometric operations, compute the dominant morphology under an antenna’s footprint, and return results via an API.

   **Technical Implementation Details:**
   - **Azure Maps Services:**
     - Get Polygon API for footprint creation
     - Data Service REST APIs for spatial operations
     - Batch processing through composite operations
     - Built-in support for various coordinate systems
   
   - **Integration Points:**
     - Azure Maps .NET SDK for API access
     - GeoJSON as primary data format
     - Azure Functions for serverless processing
     - Azure Storage for caching results

4. **PostGIS Solution**
   - **Option:** Leverage PostGIS's advanced spatial capabilities and topology support
   - **POC Idea:** Implement a .NET service using Npgsql with NetTopologySuite provider for PostGIS

   **Technical Implementation Details:**
   - **Topology Support:**
     - Native topology data type
     - TopoGeometry for maintaining topological relationships
     - Automated topology validation and cleaning
   
   - **Spatial Operations:**
     - ST_Intersection with topology support
     - GIST indexing for optimal performance
     - Native support for curved geometries (important for antenna patterns)
     - Robust spatial analysis functions

   *Note on Topology and Morphology Layer Management:*
   
   **Topology Benefits for Morphology Layers:**
   - Enforces data integrity by preventing gaps and overlaps between morphology polygons
   - Ensures shared boundaries between adjacent morphology areas remain consistent
   - Reduces data redundancy by storing common boundaries only once
   - Simplifies validation and cleanup of imported morphology data

   **Topology Support Comparison:**
   - **PostGIS:**
     - Full topology engine with explicit topology data model
     - Native support for topology rules (e.g., no gaps, no overlaps)
     - TopoGeometry type maintains relationships between features
     - Built-in topology validation and error correction tools
     - Ideal for maintaining clean morphology boundaries
   
   - **SQL Server:**
     - Basic topology validation through spatial methods
     - No native topology data model
     - Must implement gap/overlap prevention in application code
     - Requires custom solutions for boundary consistency
   
   - **NetTopologySuite:**
     - Provides topology validation operations
     - Includes overlay operations that respect topology
     - No persistent topology data model
     - Topology must be verified during each operation
   
   - **Azure Maps:**
     - No direct topology support
     - Topology validation must be handled client-side
     - Better suited for visualization than topology management

   *Given the requirement for comprehensive coverage without gaps in the morphology layer, a topology-aware solution would be beneficial. PostGIS's topology model would be particularly valuable for initial data cleanup and ongoing maintenance of the morphology layer, ensuring data quality and spatial integrity.*
