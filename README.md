# VF : Volume file format

VF is intended to provide a really easy, efficient to read & write volume data in a binary way.

## Version 2

### File Format Revisions & Four CC

- `VF2X` Revision 2 : Generic Header

### File Structure Specification

Files are structured with the following:

* **Header (77 Bytes)**
  * Four CC (4 Bytes) equal to `VF2X` All Upper Case
  * Minor Version Number (1 Byte) 
   * Data Type (1 Byte)
       * `DataType.Float = 0`
       * `DataType.Vector2 = 1`
       * `DataType.Vector3 = 2`
       * `DataType.Vector4 = 3` 
  * Usage (1 Byte)
    * `Usage.Generic = 0 `- No Specific Usage Description
    * `Usage.SDF = 1 ` - Data used as Signed Distance Field
    * `Usage.Vectors = 2` - Data used as 3D Vectors
    * `Usage.VectorsSDF = 3` - Data used as 3D Vectors + 4th channel as SDF
    * `Usage.ColorLDR = 4` - Data used as LDR sRGB Color
    * `Usage.ColorHDR = 5` - Data used as HDR linear Color.
   * Resolution, in pixels (6 Bytes), Min value : 1
      * `ushort resolution.x`
      * `ushort resolution.y`
      * `ushort resolution.z`
   * Volume Local Transform (float4x4 matrix 64 Bytes)
     * Transforms a unit Box (size 1, centered, extents in range [-0.5..0.5]).
     * Matrix defined in Column-Major
     * Storage Ordered as `a11,a12,a13,a14,a21,a22,a23,a24,a31,a32,a33,a34,a41,a42,a43,a44`
* **Data (Variable, depending on Data & Resolution)**
    - Depending of the resolution and the data 
      - `count = len(dataType)* resolution.x * resolution.y * resolution.z`
    - Elements are packed as an `array of struct`

### File format Conventions

* Binary Floating-point data must be stored as little-endian
* 3D Space Values must always be stored in Y-Up / LHS coordinate system.


## Version 1 (Legacy format)

### File Format Revisions & Four CC

- `VF_F / VF_V` Revision 1 : Initial file format supports Float fields & Vector3 fields. Data is stored into a 3D array of structured values.

### File Structure

Files are structured with the following:

 - FourCC (4 Bytes) equal to `VF_F` or `VF_V` all uppercase
  - The FourCC describes the contents of the volume. It always starts with 3 char `VF_` then another char `F` or `V` to describe if the volume contains `float` data or `vector` data.
 - Volume Size (6 Bytes)
  - 3 `ushort` describes the X,Y, & Z size of the volume, with a maximum of 65535.
 - Data (Variable, depending on FourCC & Size)
  - Depending of the type of the volume (`float` or `vector`) a succession of `size_X * size_Y * size_Z * Stride` elements where:  `stride = 1` for `float`, `stride = 3` for `vector`
  - Elements are packed as an `array of struct`, organized by X then Y then Z.

## Data Conventions of the file format

Various types are used in file format, as a reminder:
``` 
type		size	packed		sign
-----------------------------------------
char	  1 byte                 unsigned
ushort    2 bytes                unsigned
float     4 bytes                signed
vector2	  8 bytes   float[2]     signed 
vector3	 12 bytes   float[3]     signed 
vector4	 16 bytes   float[4]     signed 
```
