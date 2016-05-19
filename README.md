# VF : VectorField file format

VF is intended to provide a really easy, efficient to read & write file format in a binary way.

## File Format Revisions & FourCC

- `VF_F / VF_V` Revision 1 : Initial file format supports Float fields & Vector3 fields. Data is stored into a 3D array of structured values.

## File Structure

Files are structured with the following:

 - FourCC (4 Bytes)
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
char	  1 byte				unsigned
ushort    2 bytes				unsigned
float     4 bytes				signed
vector	 12 bytes	float[3]	signed 
```

## TODOs

- importers / exporters for more languages and tools