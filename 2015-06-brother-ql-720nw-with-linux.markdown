Downloaded .deb of CUPS driver from http://support.brother.com/g/b/downloadend.aspx?c=eu_ot&lang=en&prod=lpql720nweuk&os=130&dlid=dlfp002207_000&flang=178&type3=592

This allowed me to install `ql720nwcupswrapper-1.0.2-0.i386.deb`

Also installed LPR driver `ql720nwlpr-1.0.2-0.i386.deb`

Found CUPS 2.0.2 running at http://localhost:631/printers/

Printer is found on the network.

`QL-720NW	QL-720NW		Brother QL-720NW CUPS v1.4	Idle`

Connection: `dnssd://Brother%20QL-720NW._ipp._tcp.local/?uuid=e3248000-80ce-11db-8000-00807756c4e6`

    ➜  ~  dpkg -l  | grep Brother
    ii  printer-driver-brlaser                              3-3                                        i386         printer driver for (some) Brother laser printers
    ii  printer-driver-ptouch                               1.3-8ubuntu1                               i386         printer driver Brother P-touch label printers
    ii  ql720nwcupswrapper                                  1.0.2-0                                    i386         Brother CUPS PTouch Printer Definitions
    ii  ql720nwlpr                                          1.0.2-0                                    i386         Brother lpr Ptouch Printer Definitions
