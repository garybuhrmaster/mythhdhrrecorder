# mythhdhrrecorder
MythTV External Recorder for the SiliconDust HDHR tuners

mythhdhrrecorder is a python script which implements
the MythTV External Recorder functionality to record
from a SiliconDust HDHR tuner (4th gen or HDHR3-CC)
using tuner pooling and tuner locking.  This pooling
and locking allows multiple independent compliant
applications to select an available tuner and lock
it for its exclusive use while streaming.  Known
compliant applications include the SiliconDust HDHR
viewer/DVR project apps (including their mobile
device applications (i.e. sharing with your tablet)),
the Google Android TV Live Channels (beta) network tuner
and DVR functionality, the Channels DVR, and WMC.  Note
that MythTV when using the built-in hdhomerun tuner
configurations (which use a different API) is not
compliant.

mythhdhrrecorder uses the HDHR channel "display"
numbering.  For a US cable system, that is typically
the STB number (channel 123), and for US OTA that
is the ATSC major/minor number (channel 3.1).  This
selection numbering is specified in MythTV in the
channel table in the freqid field.

mythhdhrrecorder requires that the HDHR specific
"Channel List/Detect Channels" function be performed
on each device in order to identify the channels
that can be received/tuned on the available devices.
mythhdhrrecorder will respect channels being
disabled on a device (that device will not be
used for those disabled channels).

### Limitations:

  * This recorder depends on the External Recorder
    functionality added in 0.28.

  * This recorder depends on the functionality
    added to MythTV with ticket #12919. As of
    this writing, that patch is in fixes/0.28, but
    may not have been integrated by your packager.
    Individuals may either need to be running master,
    compile their own version of MythTV, or request
    their packager to update their package.

  * The HDHR sharing API requires exclusive use of
    a tuner for each program.  There is no ability
    to obtain the entire stream and extract
    individual programs (stations) from the stream.
    For CableCARD users, this single tuner use per
    recording is the same as the other recorders.
    For US OTA users, this could be different,
    but it is rare that two stations that one
    wishes to view/record share the same frequency
    (i.e. two different stations typically used
    two different tuners). In some other countries
    channels are more likely to share the same
    frequency, and this limitation may require
    one to acquire more HDHR tuners if one wishes
    to use this (and/or the other compliant) apps
    ability to pool/lock tuners.

  * Due to the way that the MythTV External Recorder
    operates in regards to the ability to share
    external recorders, and the inability for this
    this solution to use that method, it is necessary
    for each instance of the mythhdhrrecorder in the
    MythTV capture card configuration to have a
    unique definition.  The mythhdhrrecorder
    supports an option (--devicename) which can be
    used to create the unique definition.  The
    specified name is arbitrary, and not used
    by the recorder itself.

  * This program requires python3, and a number
    libraries.  It is useful to run it standalone
    with the --help option before configuring it
    in MythTV to determine if you have the
    necessary libraries installed.

  * MythTV will fail a recording if the recorder
    is not able to find an available tuner.  In
    some situations (to "reserve" a tuner for
    (say) your Apple TV) you may wish to create
    one (or more) fewer MythTV External Recorder
    instances than available tuners.

#### mythhdhrrecorder options
  * --hdhr {ipaddress|deviceid}
    <br>
    Specify the hdhr(s) to use (the default is to
    consider all tuners found on the local network).
    Note that only devices that can provide the
    requested channel will be considered viable for
    a particular request, so if one has a combination
    of CableCARD and OTA HDHomeRuns there is no
    specific need to specify selected devices,
    although one will want to specify different
    lineups (stations) for one or more capture cards.
  * --devicename {xxx}
    <br>
    Specify the unique devicename (external recorder requirement)

#### Setup in MythTV

  * In mythtv-setup, select "Capture cards"
  * Select "New capture card"
  * Select "External (black box) recorder" as the card type
  * Specify the executable and (usually) append an arbritary unique device
    name, for example: /home/mythtv/bin/mythhdhrrecorder --devicename 0
  * Specify the Max Recordings as 1
  * Select "Finish" to create the capture card
  * Repeat as needed for the number of tuners you want MythTV to utilize
  * Perform video source setup as usual
  * For each tuner, go to input connections and add the video source,
    and select "Fetch Channels" if the video source is newly created
    to populate the channel data (this does not work with ATSC video
    sources due to the major/minor numbering, but workarounds exist)
