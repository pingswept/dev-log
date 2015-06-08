
Rotate the PDF

    `pdftk test-label-USPS-from-Shippo.pdf rotate 1right output right.pdf`

This command magically erases all whitespace around the label.

    `pdfcrop right.pdf cropped.pdf`

Combine the two commands

    `pdftk test-label-USPS-from-Shippo.pdf rotate 1right output - | pdfcrop - fixed.pdf`

Then print to LP2844 with these settings:


    Page setup > Scale: 90%
    Page setup > Paper size: 4.00x6.00"
    Page setup > Orientation: Portrait
    Page handling > Page Scaling: none
    Page handling > Auto rotate and center not checked
    Image quality > Resolution: 203dpi
