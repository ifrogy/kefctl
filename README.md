# kefctl - a command line application for controlling KEF speakers

  This application only requires Perl 5.10.1 or newer to be installed and has no
  other dependencies. So it should run pretty much everywhere without too much
  trouble, especially on Linux.

  To my knowledge there is no documentation about the KEF control protocol. So
  every feature in kefctl had to be reverse engineered by capturing network
  traffic generated by the KEF Control mobile application talking to **KEF LSX**
  and **KEF LS50 Wireless** speakers (Firmware version 4.1).
  
  Turning the KEF LS50 Wireless on only works with the newer revision (new
  electronics), serial number after LS50W13074K24L/R2G or Nocturne Edition.

## Features

  * Turn speakers on and off
  * Change input source
  * Change volume
  * Mute and unmute speakers
  * Play and pause
  * Previous and next track
  * Standby modes
  * Inverse L/R speakers
  * Check current speaker status
  * Configure DSP settings

```
Usage: kefctl [OPTIONS]

    kefctl --status
    kefctl --volume 70
    kefctl --raise 5
    kefctl --lower 5
    kefctl --off
    kefctl --on
    kefctl --play
    kefctl -i optical
    kefctl -i bluetooth -S 20 -I on
    kefctl -H 192.168.178.52 -T 50001 -i aux
    kefctl -r 5330819b0b

  Options:
    -h, --help                  Show this summary of available options
    -H, --host <host>           Speaker host, defaults to 192.168.178.42
    -i, --input <source>        Set input source to aux, bluetooth, optical,
                                usb or wifi
    -I, --inverse <mode>        Set inverse L/R speakers to on or off
    -L, --lower <percentage>    Lower volume by X percent
    -m, --mute                  Mute speakers
    -N, --next                  Next track
    -o, --off                   Turn speakers off, the speakers can be turned
                                back on with the --on option or by setting an
                                input source with the --input option
    -O, --on                    Turn speakers on
    -p, --play                  Play or pause track
    -P, --previous              Previous track
    -r, --request <hex>         Send raw request in hex format and show response
                                (very useful for testing speaker features)
    -R, --raise <percentage>    Raise volume by X percent
    -s, --status                Show current speaker status
    -S, --standby <minutes>     Set standby time to 20, 60 or 0 (to turn standby
                                off), this option can only be used together with
                                the --input option
    -T, --port <port>           Speaker port, defaults to 50001
    -u, --unmute                Unmute speakers
    -v, --volume <percentage>   Set volume to a percentage value of 0-100, be
                                aware that every input source has its own volume
                                setting
        --version               Show version

  You can also set the KEFCTL_DEBUG environment variable to get diagnostics
  information printed to STDERR.
  ```
  ```
Usage: kefdsp [OPTIONS]

    kefdsp --status
    kefdsp --desk-mode on
    kefdsp --wall-mode off
    kefdsp --desk -3.5
    kefdsp --wall -6.0
    kefdsp --treble 0
    kefdsp --high 95
    kefdsp --low 80
    kefdsp -l 80
    kefdsp -e less
    kefdsp -H 192.168.178.52 -p 50001 -g +5.0
    kefdsp -r 5330819b0bb

  Options:
    -d, --desk <db>             Set dB value for desk mode, between -6.0 and 0
                                in steps of 0.5
    -D, --desk-mode <mode>      Set desk mode on or off
    -e, --sub-ext <mode>        Set sub extension mode to less, standard or
                                extra
    -g, --sub-gain <db>         Set dB value for sub gain, between -10.0 and
                                +10.0 in steps of 1.0
    -h, --help                  Show this summary of available options
    -H, --host <host>           Speaker host, defaults to 192.168.178.42
    -i, --high <hz>             Set Hz value for high-pass mode, between 50 and
                                120 in steps of 5
    -I, --high-pass <mode>      Set high pass mode on or off
    -l, --low <hz>              Set Hz value for sub out low-pass, between 40
                                and 250 in steps of 5
    -p, --port <port>           Speaker port, defaults to 50001
    -P, --phase <mode>          Set phase correction on or off
    -r, --request <hex>         Send raw request in hex format and show response
                                (very useful for testing speaker features)
    -s, --status                Show current DSP status
    -S, --sub-polarity <mode>   Set sub polarity to - or +
    -t, --treble <db>           Set dB value for treble trim, between -2.0 and
                                +2.0 in steps of 0.5.
    -w, --wall <db>             Set dB value for wall mode, between -6.0 and 0
        --version               Show version
                                in steps of 0.5
    -W, --wall-mode <mode>      Set wall mode on or off

  You can also set the KEFDSP_DEBUG environment variable to get diagnostics
  information printed to STDERR.
  ```

## Configuration

  Both `kefctl` and `kefdsp` will try to read the speaker host from the config
  file `~/.kefctl`.

    $ echo 192.168.178.66 > ~/.kefctl

  To detect your speakers with UPnP, you can use the script
  `tools/detect-speakers.pl`. But be aware that it will require a few extra CPAN
  modules to be installed.

## Copyright and License

  Copyright (C) 2019-2020, Sebastian Riedel.

  This program is free software, you can redistribute it and/or modify it under
  the terms of the Artistic License version 2.0
