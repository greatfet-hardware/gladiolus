# Gladiolus

a GreatFET neighbor for Software Defined Infrared (SDIR)

Required KiCad dependency:

https://github.com/greatscottgadgets/gsg-kicad-lib

If you are using git, the preferred way to install gsg-kicad-lib is to use the
submodule:

```
git submodule init && git submodule update
```

## Concept

Gladiolus is an infrared (IR) transceiver in the style of Software Defined Radio (SDR), enabling implementation of arbitrary IR protocols.  In every known over-the-air IR implementation, only the amplitude of the optical signal is modulated, so we detect/modulate the amplitude with a single ADC/DAC.

## Example Targets

* 20 to 60 kHz IR remote control systems
* IrDA up to 4 Mbps
* Bosch Integrus audio broadcast over IR (DQPSK over AM)
* proximity sensors

## Wavelength

Though many IR-over-fiber implementations use longer wavelengths, all over-the-air targets we've identified use wavelengths between 850 and 950 nm.  Gladiolus detects and produces IR in that range.

## Getting Started with Gladiolus

For receive, we have brought the signal into GNU Radio via a pipe.  So:

    mkfifo /tmp/fifo
    greatfet_sdir -r -S 10200000 -f /tmp/fifo

That's going to capture at 10.2 MSps.  In GNU Radio Companion connect a file source (pointed at /tmp/fifo with Byte type) through UChar to Float and into visualization blocks such as a QT GUI Time Sink or QT GUI Frequency Sink.  If they're too strong or weak, the gain can be controlled by a DAC.

To replay a capture:
    greatfet_sdir -S 10200000 -f <filename>

If you want to transmit from a GRC flowgraph:
    greatfet_sdir -S 10200000 -f /tmp/fifo

## AC or DC coupling

The receive photodiode is AC-coupled by default for immunity to ambient light such as sunlight, but it can be switched into DC-coupled mode.

## photodiode

We use the Everlight PD204-6B through-hole photodiode (840-1100 nm).  As an option, we have included pads for the Vishay VBPW34FASR (used in IRis) which has very high sensitivity due to large surface area but is somewhat slow.

## DAC Bypass

Many applications use On-Off Keying (OOK), so the transmit DAC is not needed.  It is possible to bypass the DAC and control the TX LED(s) with one pin.

## Inspiration

[http://onetransistor.blogspot.com/2014/12/infrared-protocol-analysis-with-pc.html](http://onetransistor.blogspot.com/2014/12/infrared-protocol-analysis-with-pc.html)

[http://winlirc.sourceforge.net/](http://winlirc.sourceforge.net/)

[https://github.com/devttys0/IRis](https://github.com/devttys0/IRis)

[http://www.analogzoo.com/2016/08/photodiode-amplifier-design/](http://www.analogzoo.com/2016/08/photodiode-amplifier-design/)

[http://defcon-wireless-village.com/speakers.html#blinded_by_the_light](http://defcon-wireless-village.com/speakers.html#blinded_by_the_light)

[http://dangerousprototypes.com/docs/USB_Infrared_Toy](http://dangerousprototypes.com/docs/USB_Infrared_Toy)

[http://www.grandideastudio.com/opticspy/](http://www.grandideastudio.com/opticspy/)

EVAL-CN0272-SDPZ is an interesting evaluation board with similar capabilities.
