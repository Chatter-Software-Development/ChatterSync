# Enclosure Model Updates

This document serves as a working history + update record of the 3D-printed model design of the Enclosure Case for the Raspberry Pi Zero W board used in the implementation of [ChatterSync](https://github.com/Chatter-Software-Development/ChatterSync).

---

> **Latest Revision:**  August 29, 2023

## Enclosure Design Revisions:

- **Slightly more generous internal clearance** to account for differences in printing tolerance. *(~1.5mm clearance around board base, ~0.75mm clearance for ports)*

- **More secure board support structure** with seating pegs to hold board in place. *(May assist in situations where board and enclosure experience vibrational effects)*. Potential future upgrade could include adding complementary concentric pegs to the *Top* part of enclosure which meet bottom pegs when parts are joined so board is completely captured.
![image](https://github.com/trevordcampbell/ChatterSync/assets/55037769/0a8e6fc6-9c5e-4718-a140-1ff9508380f9)
![image](https://github.com/trevordcampbell/ChatterSync/assets/55037769/414e71ea-a3e2-435a-9559-ce1894d59f30)

- **Added complementary lip grove** to Bottom + Top portion of enclosure to ensure snug fit between both pieces when joined.

- **Added internal cantilever joint** to provide "Snap Fit" behavior between the two enclosure pieces. *(It should be removable / reusable as well by squeezing the sides of the top)*
![image](https://github.com/trevordcampbell/ChatterSync/assets/55037769/f4444ada-6ed5-4be7-9914-9a3220e9b957)
![image](https://github.com/trevordcampbell/ChatterSync/assets/55037769/a7508e59-cd1b-4625-ab96-c2cc051df075)

### Design Parameters Used:
This design was produced according to `MMGS` unit space. The following design parameters were used in creating this design:

| Parameter | Value | Unit |
|--|--|--|
| "Board Length" | 65 | mm |
| "Board Width" | 30 | mm |
| "Board Thickness" | 1.8 | mm |
| "Standoff Inner Diameter" | 2.5 | mm |
| "Top PIN Clearance" | 8.4 | mm |
| "Bottom PIN Clearance" | 1.4 | mm |
| "Top Clearance Offset" | 2 | mm |
| "Bottom Clearance Offset" | 1 | mm |
| "Board Corner Radius" | 3 | mm |
| "Shell Wall Thickness" | 2.5 | mm |
| "Clearance to Plug Top" | 2.73 | mm |
| "Inner Housing Board Offset" | 1.5 | mm |
| "Plug Gap Offset" | 1 | mm |

## Other Notes:

- The required parts for the enclosure have been provided as `.STEP` files *(AP214 format)*, as well as the original SolidWorks `.SLDPRT` files.

- The provided `.STEP` files should be openable in most / all common CAD + 3D Modeling Programs. However the ability to edit the step-by-step construction of these files is limited.

- If greater step-by-step design capabilities are needed, this repo also contains the original *SolidWorks* Part files `(.SLDPRT)` for the Enclosure. These provided `.SLDPRT` files should be openable in *Fusion360*, and maybe some other CAD programs as well.

- The `Master` model is designed as a single part reference file, with both the top and bottom part included in this one file but as two separate bodies. The *SolidWorks* multipart `Master` file has both parts of the enclosure parametrically driven by shared design parameters. This means it is very easy to make radical design changes to the entire enclosure based on parameter changes updated to the Master file. These separate bodies were then extracted into standalone part files *(Top + Bottom)*, which are provided in this repo.

## Additional Resources:

- [Raspberry Pi Zero W Model - Design Reference](https://grabcad.com/library/raspberry-pi-zero-w-1)
