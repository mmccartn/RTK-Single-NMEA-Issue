# RTK-Single-NMEA-Issue

### Location
The traces follow urban streets into [downtown LA](https://maps.google.com/maps?q=downtown+la&hl=en&sll=37.6,-95.665&sspn=71.084365,50.888672&t=h&hnear=Downtown,+Los+Angeles,+Los+Angeles+County,+California&z=13) and pass through areas suspected to have poor GPS reception caused by tall buildings and large overpasses. 

### Hardware
I am testing with ublox [NEO 7P](https://www.u-blox.com/en/gps-modules/neo-6p/neo-7p.html) and [EVK-6t-0-001](https://u-blox.com/en/evaluation-tools-a-software/gps-evaluation-kits/evk-6-evaluation-kits.html) GPS evaluation kits, both using [ANN-MS](https://www.u-blox.com/en/positioning-antennas/active-gnss-antennas/ann-ms.html) antennas.

At least one test swapped the ublox antenna for the one built into a 2008 Buick Lucern.

#### Firmware
Our 6t devices are running ublox firmware 7.03 and the 7p chips are running version 1.00 

### Base Station
The Plate Boundry Observation Station VDCY was the closest available caster. This station is situated beside a highway about 10 to 15 miles from most of rover positions. It's [NTRIP](http://igs.bkg.bund.de/ntrip/about) stream and position information was accessed the [UNAVCO consortium](http://www.unavco.org/instrumentation/networks/status/pbo/overview/VDCY). This station is equipped with an Ashtech ASH701945B_M_SCIT antenna; detailed device information (ANTEX) for this antenna is provided by [NOAA](http://www.ngs.noaa.gov/ANTCAL/Antennas.jsp?manu=Ashtech).

Several of our tests also employ our own base station, by way of a 6t or 7p static collector running on the top of the roof of an engeering building at USC.

### Measurement Settings
On our rover devices, ubx-formatted gps measurements are captured using u-center, configured with a baud rate of 9600 kBd, a measurement period of 1000 ms, and with the following messages enabled on the serial interface: NAV_POSLLH, MSG_NAV_POSECEF, MSG_RXM_RAW, and MSG_RXM_SFRB entries. 

[pyUblox](https://github.com/tridge/pyUblox/blob/master/ublox_capture_raw.py) was also used on several occasions where the Linux was neccessary, and was configured similar to above, but having options: prefered dynamic model=automobile and PPP=off for the rover and prefered dynamic model=stationary and PPP=on for the base.

NTRIP base station streams are logged at 1 Hz, concurrent with out rover tests using RTKNAVI with default configurations.

### Post Processing Settings

Post processing of the raw GNSS measurements is achived with [RTKLIB](http://www.rtklib.com/) version 2.4.2, running in a virtual Win7 instance on a Linux host machine.

RTKCONV using the default settings was used to decompose the raw measurements into [RINEX 2.01](ftp://igscb.jpl.nasa.gov/pub/data/format/rinex2.txt) format parts: observations (OBS), GNSS satellites navigations (NAV), geostationary satellites navigations (HNAV), and the Satellite-based augmentation system records (SBS). However, only the obs file results are used for all further processing.

The NMEA lat/lon data from the ubx-formatted input files is extracted using and converted to kml using an online [web tool](http://www.h-schmidt.net/NMEA/)

RTKPOST in positioning modes: Single, DGPS, Kinetic is used to convert the OBS files to position entries. In all modes, RTKPOST was configured with elevation mask of 15 degrees, ionosphere correctuon turned off, troposhphere correction turned off, GPS ang GLO satellites enabled, and base station position set to the location provided by unavco. All other settings were left default.
