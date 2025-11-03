# Renode setup
The Raspberry Pico needs configuration files for Renode to work properly.

* On MacOS, the installation location is `/Applications/Renode.app/Contents/MacOs`
* On Linux, the location for Debian, Fedora, and Arch is `/opt/renode`
* On Windows, the location is `C://Program Files/Renode`

To add the Pico configuration files:
1. Copy `rp2040_spinlock.py` and `rp2040_divider.py` to the `scripts/pydev` directory of your Renode installation.
1. Copy `rpi_pico_rp2040_w.repl` to the `platforms/cpus` directory.

# Part 1: Invariants

## State invariants
* For all trains, there exists a sensor for which train A corresponds to sensor A.

* For all system states, if approach is high and timer is high, the barrier is never low.

* For all system states, if barrier high -> alarm high.

* For each train T, approach high <-> depart low.

* For each train T, depart high <-> approach low.

## Environment invariants
* Trains take longer than 10s from detection to crossing.

* Trains will never avoid the sensor.

* Setting time to 0 will reset the timer after 10s.

# Part 2: 
* (ringing, arms_up) -> +nb_approach 
    Will not result in any change when north-train approaches.