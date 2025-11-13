# Part 1: Invariants

## State invariants

1. For all system states, if approach is high and timer is high, the barrier is never low.

2. For all system states, if barrier high -> alarm high.

3. For each train T, approach high <-> depart low.

4. For each train T, depart high <-> approach low.

5. For all train approaches, barrier must go down after 10 seconds.

6. For all train crossings, barrier must stay down. Barrier will go down after elapsed signal sounds, never before.

7. For each train approaches <-> alarm sound.

## Environment invariants
8. Trains take longer than 10s from detection to crossing.

9. Trains will never avoid the sensor.

10. Setting time to 0 will reset the timer after 10s.

11. For all trains, there exists a sensor for which train A corresponds to sensor A.

12. A train must have approached before it departs.

13. An approach signal will always precede a depart signal, and vice versa

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
| 6      | 0         | 1        | 1                  | 0                  | I13            | 7              | I8           | I13          | 14      | 0             |
| 7      | 0         | 1        | 1                  | 1                  | I13            | I13            | I5           | I5           | 15      | 0             |
| 8      | 1         | 0        | 0                  | 0                  | I2             | I2             | I2           | I2           | I2      | 1             |
| 9      | 1         | 0        | 0                  | 1                  | I2             | I2             | I2           | I2           | I2      | 1             |
| 10     | 1         | 0        | 1                  | 0                  | I2             | I2             | I2           | I2           | I2      | 1             |
| 11     | 1         | 0        | 1                  | 1                  | I2             | I2             | I2           | I2           | I2      | 1             |
| 12     | 1         | 1        | 0                  | 0                  | I6             | I6             | I6           | I6           | I6      | 1             |
| 13     | 1         | 1        | 0                  | 1                  | 15             | I4             | I13          | 4            | 13      | 0             |
| 14     | 1         | 1        | 1                  | 0                  | I13            | 15             | 8            | I13          | 14      | 0             |
| 15     | 1         | 1        | 1                  | 1                  | I13            | I13            | 13           | 14           | 15      | 0             |

| number | invariant |
|--------|-----------|
| 16     |           |

# Part 5: New FSM
![newFSM](https://github.com/user-attachments/assets/a8e53f46-7a67-441c-8d44-4b5245dad3b1)
