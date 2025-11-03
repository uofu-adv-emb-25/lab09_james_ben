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

1. For all system states, if approach is high and timer is high, the barrier is never low.

2. For all system states, if barrier high -> alarm high.

3. For each train T, approach high <-> depart low.

4. For each train T, depart high <-> approach low.

5. For all train approaches, barrier must go down after 10 seconds.

6. For all train crossings, barrier must stay down.

7. For each train approaches <-> alarm sound.

## Environment invariants
* Trains take longer than 10s from detection to crossing.

* Trains will never avoid the sensor.

* Setting time to 0 will reset the timer after 10s.

* For all trains, there exists a sensor for which train A corresponds to sensor A.

# Part 2: Varying invariants
* (ringing, arms_up) -> +nb_approach \
    Will not result in any change when north-bound train approaches. \
    FAILS 'For all train crossings, barrier must stay down.' \

# Part 3: Check your work
* If we get to (bound-depart) from north_approach, no way to exit (bound-depart) aside from timer signal (could lead to no alarm or barrier when new train approaching). -> FIXED

# Part 4: Prove it.

| number | arms_down | alarm_on | northbound_present | southbound_present | north_approach | south_approach | north_depart | south_depart | elapsed | safety_hazard |
|--------|-----------|----------|--------------------|--------------------|----------------|----------------|--------------|--------------|---------|---------------|
| 0      | 0         | 0        | 0                  | 0                  | 6              | 5              | I4           | I4           | I7      | 0             |
| 1      | 0         | 0        | 0                  | 1                  | 7              | I4             | I4           | I4           | I7      | 1             |
| 2      | 0         | 0        | 1                  | 0                  | I4             | 7              | I4           | I4           | I7      | 1             |
| 3      | 0         | 0        | 1                  | 1                  | I4             | I4             | I6           | I6           | I7      | 1             |
| 4      | 0         | 1        | 0                  | 0                  | I7             | I7             | I4           | I4           | I7      | 0             |
| 5      | 0         | 1        | 0                  | 1                  | 7              | I4             | I4           | I6           | 13      | 1             |
| 6      | 0         | 1        | 1                  | 0                  |                |                |              |              |         |               |
| 7      | 0         | 1        | 1                  | 1                  |                |                |              |              |         |               |
| 8      | 1         | 0        | 0                  | 0                  |                |                |              |              |         |               |
| 9      | 1         | 0        | 0                  | 1                  |                |                |              |              |         |               |
| 10     | 1         | 0        | 1                  | 0                  |                |                |              |              |         |               |
| 11     | 1         | 0        | 1                  | 1                  |                |                |              |              |         |               |
| 12     | 1         | 1        | 0                  | 0                  |                |                |              |              |         |               |
| 13     | 1         | 1        | 0                  | 1                  |                |                |              |              |         |               |
| 14     | 1         | 1        | 1                  | 0                  |                |                |              |              |         |               |
| 15     | 1         | 1        | 1                  | 1                  |                |                |              |              |         |               |

| number | invariant |
|--------|-----------|
| 16     |           |