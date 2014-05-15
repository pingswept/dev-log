Using T-962A reflow oven.

Replaced masking tape with Kapton tape as suggested by guy on Instructables: http://www.instructables.com/id/T962A-SMD-Reflow-Oven-FixHack/step1/Why-So-Smelly/

With previous DGF oven and the toaster oven setup, have tried two solder alloys: SAC305 and SN100C. The SN100C solder was in a FCT Assembly paste called NL932.

SAC305 is tin (Sn) alloyed with 3% silver (Ag) and 0.5% copper (Cu).

SN100C is tin, copper, nickel, and germanium, but the exact mix is known only to Nihon Superior, the manufacturer.

Nihon Superior says that the ideal profile for SN100C P500 D4 paste is (from http://www.nihonsuperior.co.jp/english/sn100c/appraisal.html#02)

* Ramp rate of 1-2 C/s to 227 C
* Dwell above 227 C for 40-60 seconds
* Peak temperature of 235-245 C.

FCT Assembly has slightly looser specs at http://www.fctsolder.com/products/TDS/NL932%20TDS.pdf

* Ramp rate of 0.5-2 C/s to 227 C.
* Dwell above 227 for 30-90 seconds.
* Peak temperature of 239-272 C.

Can "soak" at 200-210 C for 0-20 seconds to reduce voids and 20-30 seconds to reduce tombstoning.

Analog Devices AD7608 datasheet says the max solder temp for the chip with lead-free solder is 260 C.
