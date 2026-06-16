# Data Dictionary (data/)

Generated: 2026-02-26T14:34:01+00:00

This document describes the schema, column-level statistics, and validation summaries for
the two datasets used in this project.

## Directory Overview

| File | Size | Rows | Cols | UTF-8 BOM | SHA-256 |
|---|---:|---:|---:|:---:|---|
| `data/tracking_data.csv` | 4.13 GB | 746,493 | 8 | yes | `e47e0ca3e1d25199cb17c3b819980b1beb0e27ff2ab3f8afc09d483a2b5939d3` |
| `data/event_data.csv` | 167.38 MB | 185,709 | 297 | yes | `215041b56d0342cb4d0ff33286bec8c56c0d59615c5a922250365b5307cb669b` |

## tracking_data.csv

Second Spectrum per-frame tracking export (25Hz).

Frames per match (from `MATCH_SECOND_SPECTRUM_ID`):
- `029be260-0374-4dfb-9626-e21d4d0d77cd`: 151,162 frames
- `6f02240a-4a35-447d-a77b-80ba4008e53c`: 150,233 frames
- `eb750db8-672e-4ac9-aa85-8eeb056311e1`: 148,682 frames
- `f898230f-0b1a-4947-8ad7-5ee1e05c60e0`: 148,266 frames
- `9cb6f9ac-e405-49fd-b1e0-8ed669fedee3`: 148,150 frames

Ball Z out-of-play sentinel check (from `BALL` JSON `xyz[2]`):
- Z == -10.0 frames: 290,061 (38.86%)
- Z range: -10.0 to 25.86; mean=-3.3678577963892486; p95=2.67
- In-play frames (Z != -10.0): 456,432
- In-play Z stats: min=0.0; max=25.86; mean=0.8468462553019946; p95=4.42
- Ball speed: min=0.0; max=34.85; mean=3.821867706730003
- In-play ball speed: min=0.0; max=34.85; mean=6.250651772881832

HOMEPLAYERS count per frame (heuristic via occurrences of `"optaId"`):
- n_players=10: 101,332 frames
- n_players=11: 645,161 frames

AWAYPLAYERS count per frame (heuristic via occurrences of `"optaId"`):
- n_players=10: 27,070 frames
- n_players=11: 719,423 frames

Candidate record identity keys (full-file uniqueness checks):
- (`MATCH_SECOND_SPECTRUM_ID`, `FRAMEIDX`): n_unique=746,493; is_unique=True
- (`MATCH_SECOND_SPECTRUM_ID`, `PERIOD`, `FRAMEIDX`): n_unique=746,493; is_unique=True

## Cross-File Relationships

- join_key: `MATCH_SECOND_SPECTRUM_ID`
- tracking_match_ids: 5
- events_matches_total: 379
- events_matches_overlapping: 5
- events_per_tracking_match:
  - `f898230f-0b1a-4947-8ad7-5ee1e05c60e0`: 562 events
  - `9cb6f9ac-e405-49fd-b1e0-8ed669fedee3`: 527 events
  - `029be260-0374-4dfb-9626-e21d4d0d77cd`: 517 events
  - `eb750db8-672e-4ac9-aa85-8eeb056311e1`: 472 events
  - `6f02240a-4a35-447d-a77b-80ba4008e53c`: 445 events

Columns:

| Column | Type | Description | Null % | Distinct | Examples | Top values |
|---|---|---|---:|---:|---|---|
| `MATCH_SECOND_SPECTRUM_ID` | `String` | Second Spectrum match identifier (UUID-like string) | 0.00 | 5 | 029be260-0374-4dfb-9626-e21d4d0d77cd | 029be260-0374-4dfb-9626-e21d4d0d77cd (151162); 6f02240a-4a35-447d-a77b-80ba4008e53c (150233); eb750db8-672e-4ac9-aa85-8eeb056311e1 (148682) |
| `FRAMEIDX` | `Int64` | Frame index within match (0-based) | 0.00 | 151162 | 0; 1 |  |
| `GAMECLOCK` | `Float64` | Game clock in seconds (within period) | 0.00 | 80553 | 0.0; 0.04 |  |
| `PERIOD` | `Int64` | Match period (1=1H, 2=2H; extra time may appear) | 0.00 | 2 | 1 | 2 (389129); 1 (357364) |
| `AWAYPLAYERS` | `String` | JSON array of away-team player tracking objects (one per player present) | 0.00 |  | [   {     "number": 6,     "optaId": "209036",     "playerId": "281b8199-4222-423e-a0b1-8d53a5ba63e4",     "speed": 0.000000000000000e+00...; [   {     "number": 6,     "optaId": "209036",     "playerId": "281b8199-4222-423e-a0b1-8d53a5ba63e4",     "speed": 1.800000000000000e-01... |  |
| `BALL` | `String` | JSON object with ball tracking (speed, xyz); Z=-10 sentinel indicates out-of-play | 0.00 |  | {   "speed": 2.030000000000000e+01,   "xyz": [     3.500000000000000e-01,     -1.200000000000000e-01,     3.300000000000000e-01   ] }; {   "speed": 2.006000000000000e+01,   "xyz": [     1.210000000000000e+00,     -3.300000000000000e-01,     3.800000000000000e-01   ] } |  |
| `HOMEPLAYERS` | `String` | JSON array of home-team player tracking objects (one per player present) | 0.00 |  | [   {     "number": 11,     "optaId": "444145",     "playerId": "037320d1-4b80-4301-bbb6-1fa4a1669f25",     "speed": 0.000000000000000e+0...; [   {     "number": 11,     "optaId": "444145",     "playerId": "037320d1-4b80-4301-bbb6-1fa4a1669f25",     "speed": 5.200000000000000e-0... |  |
| `LASTTOUCH` | `String` | Side that last touched ball: 'home'/'away' | 0.00 | 2 | home; away | home (381104); away (365389) |

Numeric summary:

| Column | min | p05 | p50 | p95 | max | mean | std |
|---|---:|---:|---:|---:|---:|---:|---:|
| `FRAMEIDX` | 0 | 7.46e+03 | 7.46e+04 | 1.42e+05 | 1.51e+05 | 7.47e+04 | 4.31e+04 |
| `GAMECLOCK` | 0 | 149.28 | 1.49e+03 | 2.85e+03 | 3.22e+03 | 1.5e+03 | 869.318 |
| `PERIOD` | 1 | 1 | 2 | 2 | 2 | 1.5213 | 0.4995 |

### HOMEPLAYERS JSON Schema (sample-inferred)

```json
{
  "element_keys": {
    "number": 2200,
    "optaId": 2200,
    "playerId": 2200,
    "speed": 2200,
    "xyz": 2200
  },
  "element_value_types": {
    "number": {
      "int": 2200
    },
    "optaId": {
      "str": 2200
    },
    "playerId": {
      "str": 2200
    },
    "speed": {
      "float": 2200
    },
    "xyz": {
      "list": 2200
    }
  },
  "list_lengths": {
    "11": 200
  },
  "n_parsed": 200,
  "root_keys": {},
  "root_types": {
    "list": 200
  }
}
```

### AWAYPLAYERS JSON Schema (sample-inferred)

```json
{
  "element_keys": {
    "number": 2200,
    "optaId": 2200,
    "playerId": 2200,
    "speed": 2200,
    "xyz": 2200
  },
  "element_value_types": {
    "number": {
      "int": 2200
    },
    "optaId": {
      "str": 2200
    },
    "playerId": {
      "str": 2200
    },
    "speed": {
      "float": 2200
    },
    "xyz": {
      "list": 2200
    }
  },
  "list_lengths": {
    "11": 200
  },
  "n_parsed": 200,
  "root_keys": {},
  "root_types": {
    "list": 200
  }
}
```

### BALL JSON Schema (sample-inferred)

```json
{
  "element_keys": {},
  "element_value_types": {},
  "list_lengths": {},
  "n_parsed": 200,
  "root_keys": {
    "speed": 200,
    "xyz": 200
  },
  "root_types": {
    "dict": 200
  }
}
```

## event_data.csv

Skills Corner event export (off-ball run events + associated context fields).
- n_matches: 379
- n_teams: 20
- n_players: 547

Top values for `EVENT_TYPE` (from full file):
- `off_ball_run`: 185,709

Top values for `EVENT_SUBTYPE` (from full file):
- `run_ahead_of_the_ball`: 52,956
- `support`: 30,294
- `coming_short`: 22,153
- `cross_receiver`: 20,084
- `dropping_off`: 18,671
- `behind`: 15,693
- `pulling_wide`: 11,366
- `overlap`: 5,497
- `underlap`: 4,698
- `pulling_half_space`: 4,297

Top values for `TEAM_SHORTNAME` (from full file):
- `Fulham`: 10,895
- `Newcastle`: 10,763
- `Arsenal`: 10,659
- `Tottenham`: 10,510
- `Liverpool`: 10,479
- `Manchester City`: 10,078
- `Chelsea`: 9,995
- `Manchester U`: 9,874
- `Brighton`: 9,554
- `Aston Villa`: 9,423
- `West Ham`: 9,375
- `Southampton`: 8,974

Top values for `PLAYER_POSITION` (from full file):
- `CF`: 29,139
- `AM`: 16,576
- `LW`: 15,991
- `RW`: 15,404
- `LB`: 14,163
- `RB`: 12,878
- `LM`: 11,676
- `RM`: 11,647
- `LDM`: 8,481
- `RDM`: 8,109
- `LCB`: 7,425
- `RCB`: 7,346

Top values for `ATTACKING_SIDE` (from full file):
- `right_to_left`: 93,229
- `left_to_right`: 92,480

Candidate record identity keys (full-file uniqueness checks):
- (`MATCH_SKILLCORNER_ID`, `INDEX`): n_unique=185,399; is_unique=False; dup_groups=310; extra_rows=310; max_mult=2
- (`MATCH_SKILLCORNER_ID`, `EVENT_ID`): n_unique=183,638; is_unique=False; dup_groups=2,071; extra_rows=2,071; max_mult=2
- (`MATCH_SECOND_SPECTRUM_ID`, `INDEX`): n_unique=185,399; is_unique=False; dup_groups=310; extra_rows=310; max_mult=2

Data quality quick scan:
- All-null columns (likely unused for off_ball_run; N=176):

```text
AFFECTED_LINE_BREAKING_PASSING_OPTION_ID
AFFECTED_LINE_BREAKING_PASSING_OPTION_RUN_SUBTYPE_ID
AFFECTED_LINE_BREAK_ID
ASSOCIATED_OFF_BALL_RUN_EVENT_ID
ASSOCIATED_OFF_BALL_RUN_SUBTYPE_ID
ASSOCIATED_PLAYER_POSSESSION_END_TYPE_ID
CURRENT_TEAM_IN_POSSESSION_NEXT_PHASE_TYPE_ID
CURRENT_TEAM_IN_POSSESSION_PREVIOUS_PHASE_TYPE_ID
CURRENT_TEAM_OUT_OF_POSSESSION_NEXT_PHASE_TYPE_ID
CURRENT_TEAM_OUT_OF_POSSESSION_PREVIOUS_PHASE_TYPE_ID
END_TYPE_ID
FIRST_LINE_BREAK_TYPE_ID
FURTHEST_LINE_BREAK_ID
FURTHEST_LINE_BREAK_TYPE_ID
GAME_INTERRUPTION_AFTER_ID
GAME_INTERRUPTION_BEFORE_ID
INTERPLAYER_DIRECTION_ID
INTERPLAYER_DISTANCE_RANGE_ID
LAST_LINE_BREAK_TYPE_ID
PASS_DIRECTION_ID
PASS_DIRECTION_RECEIVED_ID
PASS_OUTCOME_ID
PASS_RANGE_ID
PASS_RANGE_RECEIVED_ID
PLAYER_TARGETED_CHANNEL_PASS_ID
PLAYER_TARGETED_CHANNEL_RECEPTION_ID
PLAYER_TARGETED_ID
PLAYER_TARGETED_POSITION_ID
PLAYER_TARGETED_SPEED_AVG_BAND_ID
PLAYER_TARGETED_THIRD_PASS_ID
PLAYER_TARGETED_THIRD_RECEPTION_ID
PRESSING_CHAIN_END_TYPE_ID
SECOND_LAST_LINE_BREAK_TYPE_ID
START_TYPE_ID
TARGETED_PASSING_OPTION_EVENT_ID
AFFECTED_LINE_BREAK
AFFECTED_LINE_BREAKING_PASSING_OPTION_ATTEMPTED
AFFECTED_LINE_BREAKING_PASSING_OPTION_DANGEROUS
AFFECTED_LINE_BREAKING_PASSING_OPTION_RUN_SUBTYPE
AFFECTED_LINE_BREAKING_PASSING_OPTION_XTHREAT
ANGLE_OF_ENGAGEMENT
ASSOCIATED_OFF_BALL_RUN_SUBTYPE
ASSOCIATED_PLAYER_POSSESSION_END_TYPE
ASSOCIATED_PLAYER_POSSESSION_FRAME_END
BEATEN_BY_MOVEMENT
BEATEN_BY_POSSESSION
CARRY
CLOSE_AT_PLAYER_POSSESSION_START
CONSECUTIVE_ON_BALL_ENGAGEMENTS
CURRENT_TEAM_IN_POSSESSION_NEXT_PHASE_TYPE
CURRENT_TEAM_IN_POSSESSION_PREVIOUS_PHASE_TYPE
CURRENT_TEAM_OUT_OF_POSSESSION_NEXT_PHASE_TYPE
CURRENT_TEAM_OUT_OF_POSSESSION_PREVIOUS_PHASE_TYPE
DEFENSIVE_STRUCTURE
END_TYPE
FIRST_LINE_BREAK
FIRST_LINE_BREAK_TYPE
FIRST_PLAYER_POSSESSION_IN_TEAM_POSSESSION
FORCE_BACKWARD
FORWARD_MOMENTUM
FRAME_PHYSICAL_START
FULLY_EXTRAPOLATED
FURTHEST_LINE_BREAK
FURTHEST_LINE_BREAK_TYPE
GAME_INTERRUPTION_AFTER
GAME_INTERRUPTION_BEFORE
GOAL_SIDE_END
GOAL_SIDE_START
HAND_PASS
HIGH_PASS
INDEX_IN_PRESSING_CHAIN
INITIATE_GIVE_AND_GO
INTERPLAYER_ANGLE
INTERPLAYER_DIRECTION
INTERPLAYER_DISTANCE
INTERPLAYER_DISTANCE_END
INTERPLAYER_DISTANCE_MIN
INTERPLAYER_DISTANCE_RANGE
INTERPLAYER_DISTANCE_START
INTERPLAYER_DISTANCE_START_PHYSICAL
ISSUED_FROM_DIFFERENT_PHASE
IS_HEADER
IS_PREVIOUS_PASS_MATCHED
LAST_LINE_BREAK
LAST_LINE_BREAK_TYPE
LAST_PLAYER_POSSESSION_IN_TEAM_POSSESSION
LEAD_TO_DIFFERENT_PHASE
N_DEFENSIVE_LINES
N_OFF_BALL_RUNS
N_OPPONENTS_AHEAD_PASS_RECEPTION
N_OPPONENTS_AHEAD_PLAYER_IN_POSSESSION_PASS_MOMENT
N_OPPONENTS_BYPASSED
N_PASSING_OPTIONS
N_PASSING_OPTIONS_AHEAD
N_PASSING_OPTIONS_AHEAD_AT_END
N_PASSING_OPTIONS_AHEAD_AT_START
N_PASSING_OPTIONS_AT_END
N_PASSING_OPTIONS_AT_START
N_PASSING_OPTIONS_DANGEROUS_DIFFICULT
N_PASSING_OPTIONS_DANGEROUS_NOT_DIFFICULT
N_PASSING_OPTIONS_FIRST_LINE_BREAK
N_PASSING_OPTIONS_LAST_LINE_BREAK
N_PASSING_OPTIONS_LINE_BREAK
N_PASSING_OPTIONS_NOT_DANGEROUS_DIFFICULT
N_PASSING_OPTIONS_NOT_DANGEROUS_NOT_DIFFICULT
N_PASSING_OPTIONS_SECOND_LAST_LINE_BREAK
N_PLAYER_POSSESSIONS_IN_PHASE
N_PLAYER_TARGETED_OPPONENTS_AHEAD_END
N_PLAYER_TARGETED_OPPONENTS_AHEAD_START
N_PLAYER_TARGETED_OPPONENTS_WITHIN_5M_END
N_PLAYER_TARGETED_OPPONENTS_WITHIN_5M_START
N_PLAYER_TARGETED_TEAMMATES_AHEAD_END
N_PLAYER_TARGETED_TEAMMATES_AHEAD_START
N_PLAYER_TARGETED_TEAMMATES_WITHIN_5M_END
N_PLAYER_TARGETED_TEAMMATES_WITHIN_5M_START
N_SIMULTANEOUS_PASSING_OPTIONS
N_TEAMMATES_AHEAD_END
N_TEAMMATES_AHEAD_START
ONE_TOUCH
ORGANISED_DEFENSE
PASSING_OPTION_AT_PASS_MOMENT
PASS_AHEAD
PASS_ANGLE
PASS_ANGLE_RECEIVED
PASS_DIRECTION
PASS_DIRECTION_RECEIVED
PASS_DISTANCE
PASS_DISTANCE_RECEIVED
PASS_OUTCOME
PASS_RANGE
PASS_RANGE_RECEIVED
PEAK_PASSING_OPTION_FRAME
PLAYER_POSSESSION_PHASE_INDEX
PLAYER_TARGETED_ANGLE_TO_GOAL_END
PLAYER_TARGETED_ANGLE_TO_GOAL_START
PLAYER_TARGETED_AVERAGE_SPEED
PLAYER_TARGETED_CHANNEL_PASS
PLAYER_TARGETED_CHANNEL_RECEPTION
PLAYER_TARGETED_DANGEROUS
PLAYER_TARGETED_DIFFICULT_PASS_TARGET
PLAYER_TARGETED_DISTANCE_TO_GOAL_END
PLAYER_TARGETED_DISTANCE_TO_GOAL_START
PLAYER_TARGETED_NAME
PLAYER_TARGETED_PENALTY_AREA_PASS
PLAYER_TARGETED_PENALTY_AREA_RECEPTION
PLAYER_TARGETED_POSITION
PLAYER_TARGETED_SPEED_AVG_BAND
PLAYER_TARGETED_THIRD_PASS
PLAYER_TARGETED_THIRD_RECEPTION
PLAYER_TARGETED_XPASS_COMPLETION
PLAYER_TARGETED_XTHREAT
PLAYER_TARGETED_X_PASS
PLAYER_TARGETED_X_RECEPTION
PLAYER_TARGETED_Y_PASS
PLAYER_TARGETED_Y_RECEPTION
POSSESSION_DANGER
PRESSING_CHAIN
PRESSING_CHAIN_END_TYPE
PRESSING_CHAIN_INDEX
PRESSING_CHAIN_LENGTH
QUICK_PASS
REDUCE_POSSESSION_DANGER
SECOND_LAST_LINE_BREAK
SECOND_LAST_LINE_BREAK_TYPE
SIMULTANEOUS_DEFENSIVE_ENGAGEMENT_SAME_TARGET
SIMULTANEOUS_DEFENSIVE_ENGAGEMENT_SAME_TARGET_RANK
SPEED_DIFFERENCE
START_TYPE
STOP_POSSESSION_DANGER
TEAM_POSSESSION_LOSS_IN_PHASE
XLOSS_PLAYER_POSSESSION_END
XLOSS_PLAYER_POSSESSION_MAX
XLOSS_PLAYER_POSSESSION_START
XSHOT_PLAYER_POSSESSION_END
XSHOT_PLAYER_POSSESSION_MAX
XSHOT_PLAYER_POSSESSION_START
```
- Most-missing columns:
  - `AFFECTED_LINE_BREAKING_PASSING_OPTION_ID`: 100.00% null
  - `AFFECTED_LINE_BREAKING_PASSING_OPTION_RUN_SUBTYPE_ID`: 100.00% null
  - `AFFECTED_LINE_BREAK_ID`: 100.00% null
  - `ASSOCIATED_OFF_BALL_RUN_EVENT_ID`: 100.00% null
  - `ASSOCIATED_OFF_BALL_RUN_SUBTYPE_ID`: 100.00% null
  - `ASSOCIATED_PLAYER_POSSESSION_END_TYPE_ID`: 100.00% null
  - `CURRENT_TEAM_IN_POSSESSION_NEXT_PHASE_TYPE_ID`: 100.00% null
  - `CURRENT_TEAM_IN_POSSESSION_PREVIOUS_PHASE_TYPE_ID`: 100.00% null
  - `CURRENT_TEAM_OUT_OF_POSSESSION_NEXT_PHASE_TYPE_ID`: 100.00% null
  - `CURRENT_TEAM_OUT_OF_POSSESSION_PREVIOUS_PHASE_TYPE_ID`: 100.00% null
  - `END_TYPE_ID`: 100.00% null
  - `FIRST_LINE_BREAK_TYPE_ID`: 100.00% null
  - `FURTHEST_LINE_BREAK_ID`: 100.00% null
  - `FURTHEST_LINE_BREAK_TYPE_ID`: 100.00% null
  - `GAME_INTERRUPTION_AFTER_ID`: 100.00% null
  - `GAME_INTERRUPTION_BEFORE_ID`: 100.00% null
  - `INTERPLAYER_DIRECTION_ID`: 100.00% null
  - `INTERPLAYER_DISTANCE_RANGE_ID`: 100.00% null
  - `LAST_LINE_BREAK_TYPE_ID`: 100.00% null
  - `PASS_DIRECTION_ID`: 100.00% null

Columns (grouped):

### Match

| Column | Type | Description | Null % | Distinct | Examples | Top values |
|---|---|---|---:|---:|---|---|
| `MATCH_SECOND_SPECTRUM_ID` | `String` | Second Spectrum match identifier (links to tracking file) | 0.00 | 379 | a4417606-8767-447e-b016-c0868b4bca33; de40f1d9-2af2-4711-ac08-5b432894bc55 | de6ed574-561c-4256-9be8-162e699173d7 (1039); 4362921c-97d6-4187-958e-6009b6d02cf6 (1018); b3b47041-95d0-48cd-a01d-f01e2c0738b1 (948) |
| `MATCH_SKILLCORNER_ID` | `Int64` | SkillsCorner match identifier | 0.00 | 379 | 1650385; 1650961 | 1806021 (1039); 1832950 (1018); 1747407 (948) |
| `MATCH_ID` | `Int64` | Identifier code | 0.00 | 379 | 1650385; 1650961 | 1806021 (1039); 1832950 (1018); 1747407 (948) |

Numeric summary:

| Column | min | p05 | p50 | p95 | max | mean | std |
|---|---:|---:|---:|---:|---:|---:|---:|
| `MATCH_SKILLCORNER_ID` | 1.65e+06 | 1.67e+06 | 1.95e+06 | 2.02e+06 | 2.02e+06 | 1.91e+06 | 1.13e+05 |
| `MATCH_ID` | 1.65e+06 | 1.67e+06 | 1.95e+06 | 2.02e+06 | 2.02e+06 | 1.91e+06 | 1.13e+05 |

### Event

| Column | Type | Description | Null % | Distinct | Examples | Top values |
|---|---|---|---:|---:|---|---|
| `EVENT_ID` | `String` | Event identifier (string, often '<match>_<idx>') | 0.00 | 638 | 1_0; 1_1 | 1_104 (384); 1_161 (384); 1_35 (384) |
| `EVENT_SUBTYPE_ID` | `Int64` | Identifier code | 0.00 | 10 | 2; 9 | 8 (52956); 9 (30294); 2 (22153) |
| `EVENT_TYPE_ID` | `Int64` | Identifier code | 0.00 | 1 | 1 | 1 (185709) |
| `EVENT_SUBTYPE` | `String` | Event subtype | 0.00 | 10 | coming_short; support | run_ahead_of_the_ball (52956); support (30294); coming_short (22153) |
| `EVENT_TYPE` | `String` | High-level event type | 0.00 | 1 | off_ball_run | off_ball_run (185709) |

Numeric summary:

| Column | min | p05 | p50 | p95 | max | mean | std |
|---|---:|---:|---:|---:|---:|---:|---:|
| `EVENT_SUBTYPE_ID` | 1 | 1 | 7 | 9 | 10 | 5.7673 | 2.9064 |
| `EVENT_TYPE_ID` | 1 | 1 | 1 | 1 | 1 | 1 | 0 |

### Player

| Column | Type | Description | Null % | Distinct | Examples | Top values |
|---|---|---|---:|---:|---|---|
| `PLAYER_ID` | `Int64` | Primary player identifier | 0.00 | 547 | 12941; 5794 | 24870 (1448); 24053 (1436); 26154 (1347) |
| `PLAYER_IN_POSSESSION_ID` | `Int64` | Identifier code | 0.00 | 549 | 13068; 12168 | 7735 (1642); 494 (1459); 12629 (1420) |
| `PLAYER_IN_POSSESSION_POSITION_ID` | `Int64` | Identifier code | 0.00 | 19 | 12; 17 | 19 (17330); 16 (16664); 17 (15903) |
| `PLAYER_POSITION_ID` | `Int64` | Identifier code | 0.00 | 19 | 19; 8 | 19 (29139); 15 (16576); 16 (15991) |
| `PLAYER_TARGETED_CHANNEL_PASS_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_CHANNEL_RECEPTION_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_POSITION_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_SPEED_AVG_BAND_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_THIRD_PASS_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_THIRD_RECEPTION_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `PLAYER_IN_POSSESSION_CHANNEL_END` | `String` | End-state attribute | 0.00 | 5 | center; wide_right |  |
| `PLAYER_IN_POSSESSION_CHANNEL_ID_END` | `Int64` | End-state attribute | 0.00 | 5 | 3; 5 |  |
| `PLAYER_IN_POSSESSION_CHANNEL_ID_START` | `Int64` | Start-state attribute | 0.00 | 5 | 3; 5 |  |
| `PLAYER_IN_POSSESSION_CHANNEL_START` | `String` | Start-state attribute | 0.00 | 5 | center; wide_right |  |
| `PLAYER_IN_POSSESSION_NAME` | `String` | Human-readable name | 0.00 | 546 | S. Lukić; Adama Traoré | Mohamed Salah (1642); A. Iwobi (1459); B. Mbeumo (1420) |
| `PLAYER_IN_POSSESSION_PENALTY_AREA_END` | `Boolean` | End-state attribute | 0.00 | 2 | False; True |  |
| `PLAYER_IN_POSSESSION_PENALTY_AREA_START` | `Boolean` | Start-state attribute | 0.00 | 2 | False; True |  |
| `PLAYER_IN_POSSESSION_POSITION` | `String` |  | 0.00 | 19 | LM; RW | CF (17330); LW (16664); RW (15903) |
| `PLAYER_IN_POSSESSION_THIRD_END` | `String` | End-state attribute | 0.00 | 3 | middle_third; defensive_third |  |
| `PLAYER_IN_POSSESSION_THIRD_ID_END` | `Int64` | End-state attribute | 0.00 | 3 | 2; 1 |  |
| `PLAYER_IN_POSSESSION_THIRD_ID_START` | `Int64` | Start-state attribute | 0.00 | 3 | 2; 1 |  |
| `PLAYER_IN_POSSESSION_THIRD_START` | `String` | Start-state attribute | 0.00 | 3 | middle_third; defensive_third |  |
| `PLAYER_IN_POSSESSION_X_END` | `Float64` | End-state attribute | 0.00 | 10637 | -9.72; 10.91 |  |
| `PLAYER_IN_POSSESSION_X_START` | `Float64` | Start-state attribute | 0.00 | 10175 | -9.58; 9.35 |  |
| `PLAYER_IN_POSSESSION_Y_END` | `Float64` | End-state attribute | 0.00 | 7120 | -1.36; -26.0 |  |
| `PLAYER_IN_POSSESSION_Y_START` | `Float64` | Start-state attribute | 0.00 | 7020 | -0.1; -32.52 |  |
| `PLAYER_NAME` | `String` | Primary player name | 0.00 | 544 | E. Smith Rowe; K. Tete | Bruno Guimarães (1448); D. Kulusevski (1436); C. Palmer (1347) |
| `PLAYER_POSITION` | `String` | Primary player position label (metadata; may be noisy) | 0.00 | 19 | CF; RB | CF (29139); AM (16576); LW (15991) |
| `PLAYER_POSSESSION_PHASE_INDEX` | `String` |  | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_ANGLE_TO_GOAL_END` | `String` | End-state attribute | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_ANGLE_TO_GOAL_START` | `String` | Start-state attribute | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_AVERAGE_SPEED` | `String` |  | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_CHANNEL_PASS` | `String` |  | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_CHANNEL_RECEPTION` | `String` |  | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_DANGEROUS` | `String` |  | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_DIFFICULT_PASS_TARGET` | `String` |  | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_DISTANCE_TO_GOAL_END` | `String` | End-state attribute | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_DISTANCE_TO_GOAL_START` | `String` | Start-state attribute | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_NAME` | `String` | Human-readable name | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_PENALTY_AREA_PASS` | `String` |  | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_PENALTY_AREA_RECEPTION` | `String` |  | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_POSITION` | `String` |  | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_SPEED_AVG_BAND` | `String` |  | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_THIRD_PASS` | `String` |  | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_THIRD_RECEPTION` | `String` |  | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_XPASS_COMPLETION` | `String` |  | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_XTHREAT` | `String` |  | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_X_PASS` | `String` |  | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_X_RECEPTION` | `String` |  | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_Y_PASS` | `String` |  | 100.00 | 1 |  |  |
| `PLAYER_TARGETED_Y_RECEPTION` | `String` |  | 100.00 | 1 |  |  |

Numeric summary:

| Column | min | p05 | p50 | p95 | max | mean | std |
|---|---:|---:|---:|---:|---:|---:|---:|
| `PLAYER_ID` | 82 | 2.32e+03 | 1.8e+04 | 1.17e+05 | 1e+06 | 3.85e+04 | 9.46e+04 |
| `PLAYER_IN_POSSESSION_ID` | 82 | 2.31e+03 | 1.65e+04 | 1.17e+05 | 1e+06 | 3.7e+04 | 9.12e+04 |
| `PLAYER_IN_POSSESSION_POSITION_ID` | 1 | 1 | 11 | 19 | 20 | 10.8566 | 6.1282 |
| `PLAYER_POSITION_ID` | 1 | 2 | 14 | 19 | 20 | 12.4697 | 5.6318 |
| `PLAYER_IN_POSSESSION_CHANNEL_ID_END` | 1 | 1 | 3 | 5 | 5 | 2.9802 | 1.441 |
| `PLAYER_IN_POSSESSION_CHANNEL_ID_START` | 1 | 1 | 3 | 5 | 5 | 2.9771 | 1.4381 |
| `PLAYER_IN_POSSESSION_THIRD_ID_END` | 1 | 1 | 2 | 3 | 3 | 2.2055 | 0.7441 |
| `PLAYER_IN_POSSESSION_THIRD_ID_START` | 1 | 1 | 2 | 3 | 3 | 2.0574 | 0.7208 |
| `PLAYER_IN_POSSESSION_X_END` | -55.59 | -39.05 | 9.25 | 44.4 | 57.24 | 7.0635 | 25.5935 |
| `PLAYER_IN_POSSESSION_X_START` | -54.3 | -39.33 | 2.76 | 35.8 | 56.7 | 1.3289 | 23.2092 |
| `PLAYER_IN_POSSESSION_Y_END` | -41.1 | -30.35 | 0.51 | 30.45 | 43.7 | 0.2851 | 20.0979 |
| `PLAYER_IN_POSSESSION_Y_START` | -41.25 | -30.71 | 0.47 | 31.01 | 38.07 | 0.353 | 20.1993 |

### Team

| Column | Type | Description | Null % | Distinct | Examples | Top values |
|---|---|---|---:|---:|---|---|
| `TEAM_ID` | `Int64` | Team identifier | 0.00 | 20 | 48; 31 | 48 (10895); 32 (10763); 3 (10659) |
| `TEAM_IN_POSSESSION_PHASE_TYPE_ID` | `Int64` | Identifier code | 0.00 | 9 | 1; 5 | 1 (62235); 2 (61761); 0 (21394) |
| `TEAM_OUT_OF_POSSESSION_PHASE_TYPE_ID` | `Int64` | Identifier code | 0.00 | 9 | 9; 5 | 9 (62235); 8 (61761); 10 (21394) |
| `TEAM_IN_POSSESSION_PHASE_TYPE` | `String` |  | 0.00 | 9 | create; chaotic | create (62235); finish (61761); build_up (21394) |
| `TEAM_OUT_OF_POSSESSION_PHASE_TYPE` | `String` |  | 0.00 | 9 | medium_block; chaotic | medium_block (62235); low_block (61761); high_block (21394) |
| `TEAM_POSSESSION_LOSS_IN_PHASE` | `String` |  | 100.00 | 1 |  |  |
| `TEAM_SCORE` | `Int64` |  | 0.00 | 8 | 0; 1 |  |
| `TEAM_SHORTNAME` | `String` | Team short name | 0.00 | 20 | Fulham; Manchester U | Fulham (10895); Newcastle (10763); Arsenal (10659) |

Numeric summary:

| Column | min | p05 | p50 | p95 | max | mean | std |
|---|---:|---:|---:|---:|---:|---:|---:|
| `TEAM_ID` | 2 | 2 | 48 | 752 | 754 | 146.2685 | 241.5529 |
| `TEAM_IN_POSSESSION_PHASE_TYPE_ID` | 0 | 0 | 2 | 6 | 14 | 2.0788 | 1.8539 |
| `TEAM_OUT_OF_POSSESSION_PHASE_TYPE_ID` | 5 | 8 | 9 | 13 | 15 | 9.1911 | 1.8639 |
| `TEAM_SCORE` | 0 | 0 | 0 | 3 | 7 | 0.6686 | 0.9532 |

### Coordinates

| Column | Type | Description | Null % | Distinct | Examples | Top values |
|---|---|---|---:|---:|---|---|
| `X_END` | `Float64` | End X coordinate (meters; attacking reference frame) | 0.00 | 9939 | -6.23; 3.55 |  |
| `X_START` | `Float64` | Start X coordinate (meters; attacking reference frame) | 0.00 | 9689 | -2.99; -12.74 |  |
| `Y_END` | `Float64` | End Y coordinate (meters; attacking reference frame) | 0.00 | 6969 | 2.77; -28.08 |  |
| `Y_START` | `Float64` | Start Y coordinate (meters; attacking reference frame) | 0.00 | 6994 | 8.19; -29.71 |  |

Numeric summary:

| Column | min | p05 | p50 | p95 | max | mean | std |
|---|---:|---:|---:|---:|---:|---:|---:|
| `X_END` | -53.21 | -29.27 | 16.45 | 43.2 | 57.12 | 12.6379 | 23.1125 |
| `X_START` | -51.68 | -30.35 | 6.02 | 35.53 | 56.29 | 4.6457 | 20.3313 |
| `Y_END` | -38.84 | -28.06 | 0.35 | 28.43 | 41.22 | 0.216 | 17.9578 |
| `Y_START` | -37.89 | -27.13 | 0.35 | 27.76 | 45 | 0.3548 | 16.6211 |

### Start/End

| Column | Type | Description | Null % | Distinct | Examples | Top values |
|---|---|---|---:|---:|---|---|
| `TIME_END` | `String` | End timestamp (mm:ss.s) | 0.00 | 55465 | 00:06.2; 00:14.6 | 45:33.8 (21); 00:04.2 (21); 45:06.3 (18) |
| `TIME_START` | `String` | Start timestamp (mm:ss.s) | 0.00 | 55270 | 00:04.9; 00:11.5 | 45:02.5 (20); 45:01.7 (18); 00:02.2 (18) |
| `CHANNEL_END` | `String` | End pitch channel label | 0.00 | 5 | center; wide_right | center (55910); half_space_left (34079); half_space_right (33463) |
| `CHANNEL_ID_END` | `Int64` | End-state attribute | 0.00 | 5 | 3; 5 |  |
| `CHANNEL_ID_START` | `Int64` | Start-state attribute | 0.00 | 5 | 3; 5 |  |
| `CHANNEL_START` | `String` | Start pitch channel label (e.g., wide_left/half_space/etc) | 0.00 | 5 | center; wide_right | center (67027); half_space_left (35290); half_space_right (34303) |
| `CLOSE_AT_PLAYER_POSSESSION_START` | `String` | Start-state attribute | 100.00 | 1 |  |  |
| `DELTA_TO_LAST_DEFENSIVE_LINE_END` | `Float64` | End-state attribute | 0.00 | 5583 | 25.55; 15.05 |  |
| `DELTA_TO_LAST_DEFENSIVE_LINE_START` | `Float64` | Start-state attribute | 0.00 | 5203 | 22.59; 26.42 |  |
| `DISTANCE_TO_PLAYER_IN_POSSESSION_END` | `Float64` | End-state attribute | 0.00 | 5170 | 5.41; 7.65 |  |
| `DISTANCE_TO_PLAYER_IN_POSSESSION_START` | `Float64` | Start-state attribute | 0.00 | 5460 | 10.59; 22.27 |  |
| `FRAME_END` | `Int64` | End frame index (event timeline) | 0.00 | 58184 | 72; 156 |  |
| `FRAME_PHYSICAL_START` | `String` | Start-state attribute | 100.00 | 1 |  |  |
| `FRAME_START` | `Int64` | Start frame index (event timeline) | 0.00 | 57990 | 59; 125 |  |
| `GOAL_SIDE_END` | `String` | End-state attribute | 100.00 | 1 |  |  |
| `GOAL_SIDE_START` | `String` | Start-state attribute | 100.00 | 1 |  |  |
| `INSIDE_DEFENSIVE_SHAPE_END` | `Boolean` | End-state attribute | 0.00 | 2 | True; False |  |
| `INSIDE_DEFENSIVE_SHAPE_START` | `Boolean` | Start-state attribute | 0.00 | 2 | False; True |  |
| `INTERPLAYER_DISTANCE_END` | `String` | End-state attribute | 100.00 | 1 |  |  |
| `INTERPLAYER_DISTANCE_START` | `String` | Start-state attribute | 100.00 | 1 |  |  |
| `LAST_DEFENSIVE_LINE_HEIGHT_END` | `Float64` | End-state attribute | 0.00 | 6899 | 33.18; 33.9 |  |
| `LAST_DEFENSIVE_LINE_HEIGHT_START` | `Float64` | Start-state attribute | 0.00 | 7504 | 32.9; 38.82 |  |
| `LAST_DEFENSIVE_LINE_X_END` | `Float64` | Last defensive line X at end (meters) | 0.00 | 7016 | 19.32; 18.6 |  |
| `LAST_DEFENSIVE_LINE_X_START` | `Float64` | Last defensive line X at start (meters) | 0.00 | 7540 | 19.6; 13.68 |  |
| `LOCATION_TO_PLAYER_IN_POSSESSION_END` | `String` | End-state attribute | 0.00 | 3 | ahead; behind |  |
| `LOCATION_TO_PLAYER_IN_POSSESSION_ID_END` | `Int64` | End-state attribute | 0.00 | 3 | 3; 1 |  |
| `LOCATION_TO_PLAYER_IN_POSSESSION_ID_START` | `Int64` | Start-state attribute | 0.00 | 3 | 3; 1 |  |
| `LOCATION_TO_PLAYER_IN_POSSESSION_START` | `String` | Start-state attribute | 0.00 | 3 | ahead; behind |  |
| `MINUTE_START` | `Int64` | Start minute (integer, match clock) | 0.00 | 106 | 0; 1 |  |
| `N_OPPONENTS_AHEAD_END` | `Int64` | End-state attribute | 0.00 | 11 | 9; 6 |  |
| `N_OPPONENTS_AHEAD_START` | `Int64` | Start-state attribute | 0.00 | 11 | 10; 8 |  |
| `N_PASSING_OPTIONS_AHEAD_AT_END` | `String` | End-state attribute | 100.00 | 1 |  |  |
| `N_PASSING_OPTIONS_AHEAD_AT_START` | `String` | Start-state attribute | 100.00 | 1 |  |  |
| `N_PASSING_OPTIONS_AT_END` | `String` | End-state attribute | 100.00 | 1 |  |  |
| `N_PASSING_OPTIONS_AT_START` | `String` | Start-state attribute | 100.00 | 1 |  |  |
| `N_PLAYER_TARGETED_OPPONENTS_AHEAD_END` | `String` | End-state attribute | 100.00 | 1 |  |  |
| `N_PLAYER_TARGETED_OPPONENTS_AHEAD_START` | `String` | Start-state attribute | 100.00 | 1 |  |  |
| `N_PLAYER_TARGETED_OPPONENTS_WITHIN_5M_END` | `String` | End-state attribute | 100.00 | 1 |  |  |
| `N_PLAYER_TARGETED_OPPONENTS_WITHIN_5M_START` | `String` | Start-state attribute | 100.00 | 1 |  |  |
| `N_PLAYER_TARGETED_TEAMMATES_AHEAD_END` | `String` | End-state attribute | 100.00 | 1 |  |  |
| `N_PLAYER_TARGETED_TEAMMATES_AHEAD_START` | `String` | Start-state attribute | 100.00 | 1 |  |  |
| `N_PLAYER_TARGETED_TEAMMATES_WITHIN_5M_END` | `String` | End-state attribute | 100.00 | 1 |  |  |
| `N_PLAYER_TARGETED_TEAMMATES_WITHIN_5M_START` | `String` | Start-state attribute | 100.00 | 1 |  |  |
| `N_TEAMMATES_AHEAD_END` | `String` | End-state attribute | 100.00 | 1 |  |  |
| `N_TEAMMATES_AHEAD_START` | `String` | Start-state attribute | 100.00 | 1 |  |  |
| `PASSING_OPTION_AT_PLAYER_POSSESSION_START` | `Boolean` | Start-state attribute | 0.00 | 2 | True; False |  |
| `PASSING_OPTION_AT_START` | `Boolean` | Start-state attribute | 0.00 | 2 | True; False |  |
| `PENALTY_AREA_END` | `Boolean` | Boolean: end point inside penalty area | 0.00 | 2 | False; True |  |
| `PENALTY_AREA_START` | `Boolean` | Boolean: start point inside penalty area | 0.00 | 2 | False; True |  |
| `SECOND_START` | `Int64` | Start second within minute | 0.00 | 60 | 4; 11 |  |
| `SEPARATION_END` | `Float64` | Separation at end (meters) | 0.00 | 2227 | 1.95; 4.9 |  |
| `SEPARATION_START` | `Float64` | Separation at start (meters) | 0.00 | 2454 | 1.58; 4.65 |  |
| `THIRD_END` | `String` | End pitch third label | 0.00 | 3 | middle_third; attacking_third | attacking_third (90489); middle_third (70366); defensive_third (24854) |
| `THIRD_ID_END` | `Int64` | End-state attribute | 0.00 | 3 | 2; 3 |  |
| `THIRD_ID_START` | `Int64` | Start-state attribute | 0.00 | 3 | 2; 1 |  |
| `THIRD_START` | `String` | Start pitch third label | 0.00 | 3 | middle_third; defensive_third | middle_third (98502); attacking_third (56078); defensive_third (31129) |
| `XLOSS_PLAYER_POSSESSION_END` | `String` | End-state attribute | 100.00 | 1 |  |  |
| `XLOSS_PLAYER_POSSESSION_START` | `String` | Start-state attribute | 100.00 | 1 |  |  |
| `XSHOT_PLAYER_POSSESSION_END` | `String` | End-state attribute | 100.00 | 1 |  |  |
| `XSHOT_PLAYER_POSSESSION_START` | `String` | Start-state attribute | 100.00 | 1 |  |  |

Numeric summary:

| Column | min | p05 | p50 | p95 | max | mean | std |
|---|---:|---:|---:|---:|---:|---:|---:|
| `CHANNEL_ID_END` | 1 | 1 | 3 | 5 | 5 | 2.9868 | 1.3056 |
| `CHANNEL_ID_START` | 1 | 1 | 3 | 5 | 5 | 2.9748 | 1.1964 |
| `DELTA_TO_LAST_DEFENSIVE_LINE_END` | -13.31 | -0.19 | 8.42 | 34.02 | 68.77 | 11.5389 | 10.8135 |
| `DELTA_TO_LAST_DEFENSIVE_LINE_START` | -9.62 | 1.35 | 12.32 | 32.6 | 68.71 | 14.0254 | 9.9111 |
| `DISTANCE_TO_PLAYER_IN_POSSESSION_END` | 0.09 | 5.14 | 13.92 | 31 | 84.84 | 15.4477 | 8.1673 |
| `DISTANCE_TO_PLAYER_IN_POSSESSION_START` | 0.09 | 6.14 | 16.98 | 35.51 | 73.2 | 18.3061 | 9.1031 |
| `FRAME_END` | 29 | 2.46e+03 | 2.86e+04 | 5.68e+04 | 8.01e+04 | 2.9e+04 | 1.75e+04 |
| `FRAME_START` | 12 | 2.43e+03 | 2.86e+04 | 5.68e+04 | 8e+04 | 2.89e+04 | 1.75e+04 |
| `LAST_DEFENSIVE_LINE_HEIGHT_END` | 0 | 5.96 | 26.66 | 51.99 | 86.91 | 27.934 | 15.2886 |
| `LAST_DEFENSIVE_LINE_HEIGHT_START` | 0 | 10.8 | 33.53 | 55.49 | 90.51 | 33.4391 | 14.493 |
| `LAST_DEFENSIVE_LINE_X_END` | -34.41 | 0.27 | 25.45 | 46.19 | 58.78 | 24.1768 | 15.279 |
| `LAST_DEFENSIVE_LINE_X_START` | -38.01 | -3.31 | 18.57 | 41.36 | 56.99 | 18.671 | 14.4802 |
| `LOCATION_TO_PLAYER_IN_POSSESSION_ID_END` | 1 | 1 | 3 | 3 | 3 | 2.4267 | 0.7857 |
| `LOCATION_TO_PLAYER_IN_POSSESSION_ID_START` | 1 | 1 | 2 | 3 | 3 | 2.1625 | 0.8777 |
| `MINUTE_START` | 0 | 3 | 45 | 90 | 110 | 45.7192 | 27.502 |
| `N_OPPONENTS_AHEAD_END` | 0 | 0 | 5 | 10 | 10 | 4.9365 | 2.8528 |
| `N_OPPONENTS_AHEAD_START` | 0 | 2 | 6 | 10 | 10 | 5.729 | 2.4977 |
| `SECOND_START` | 0 | 2 | 29 | 56 | 59 | 29.3729 | 17.3849 |
| `SEPARATION_END` | 0.02 | 1.18 | 4.25 | 12.2 | 33.62 | 5.1379 | 3.4848 |
| `SEPARATION_START` | 0.02 | 1.3 | 4.49 | 13.42 | 36.18 | 5.5384 | 3.8537 |
| `THIRD_ID_END` | 1 | 1 | 2 | 3 | 3 | 2.3534 | 0.7044 |
| `THIRD_ID_START` | 1 | 1 | 2 | 3 | 3 | 2.1343 | 0.672 |

### Associations

| Column | Type | Description | Null % | Distinct | Examples | Top values |
|---|---|---|---:|---:|---|---|
| `ASSOCIATED_OFF_BALL_RUN_EVENT_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `ASSOCIATED_OFF_BALL_RUN_SUBTYPE_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `ASSOCIATED_PLAYER_POSSESSION_END_TYPE_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `ASSOCIATED_PLAYER_POSSESSION_EVENT_ID` | `String` | Identifier code | 0.00 | 1167 | 8_1; 8_3 | 8_1 (360); 8_6 (256); 8_619 (248) |
| `ASSOCIATED_OFF_BALL_RUN_SUBTYPE` | `String` | Foreign-key-like reference to an associated event/entity | 100.00 | 1 |  |  |
| `ASSOCIATED_PLAYER_POSSESSION_END_TYPE` | `String` | Foreign-key-like reference to an associated event/entity | 100.00 | 1 |  |  |
| `ASSOCIATED_PLAYER_POSSESSION_FRAME_END` | `String` | Foreign-key-like reference to an associated event/entity | 100.00 | 1 |  |  |
| `ASSOCIATED_PLAYER_POSSESSION_FRAME_START` | `Int64` | Foreign-key-like reference to an associated event/entity | 0.00 | 53686 | 62; 143 |  |

Numeric summary:

| Column | min | p05 | p50 | p95 | max | mean | std |
|---|---:|---:|---:|---:|---:|---:|---:|
| `ASSOCIATED_PLAYER_POSSESSION_FRAME_START` | 15 | 2.43e+03 | 2.86e+04 | 5.68e+04 | 8.01e+04 | 2.89e+04 | 1.75e+04 |

### Other

| Column | Type | Description | Null % | Distinct | Examples | Top values |
|---|---|---|---:|---:|---|---|
| `PERIOD` | `Int64` | Match period | 0.00 | 2 | 1; 2 | 1 (94121); 2 (91588) |
| `AFFECTED_LINE_BREAKING_PASSING_OPTION_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `AFFECTED_LINE_BREAKING_PASSING_OPTION_RUN_SUBTYPE_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `AFFECTED_LINE_BREAK_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `ATTACKING_SIDE_ID` | `Int64` | Identifier code | 0.00 | 2 | 2; 1 | 2 (93229); 1 (92480) |
| `CURRENT_TEAM_IN_POSSESSION_NEXT_PHASE_TYPE_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `CURRENT_TEAM_IN_POSSESSION_PREVIOUS_PHASE_TYPE_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `CURRENT_TEAM_OUT_OF_POSSESSION_NEXT_PHASE_TYPE_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `CURRENT_TEAM_OUT_OF_POSSESSION_PREVIOUS_PHASE_TYPE_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `END_TYPE_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `FIRST_LINE_BREAK_TYPE_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `FURTHEST_LINE_BREAK_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `FURTHEST_LINE_BREAK_TYPE_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `GAME_INTERRUPTION_AFTER_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `GAME_INTERRUPTION_BEFORE_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `GAME_STATE_ID` | `Int64` | Identifier code | 0.00 | 3 | 3; 2 | 3 (82549); 2 (55239); 1 (47921) |
| `INTERPLAYER_DIRECTION_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `INTERPLAYER_DISTANCE_RANGE_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `LAST_LINE_BREAK_TYPE_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `PASS_DIRECTION_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `PASS_DIRECTION_RECEIVED_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `PASS_OUTCOME_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `PASS_RANGE_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `PASS_RANGE_RECEIVED_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `PRESSING_CHAIN_END_TYPE_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `SECOND_LAST_LINE_BREAK_TYPE_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `SPEED_AVG_BAND_ID` | `Int64` | Identifier code | 2.07 | 4 | 2; 3 | 2 (131062); 3 (42598); 4 (8204) |
| `START_TYPE_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `TARGETED_PASSING_OPTION_EVENT_ID` | `String` | Identifier code | 100.00 | 1 |  |  |
| `TRAJECTORY_DIRECTION_ID` | `Int64` | Identifier code | 0.00 | 4 | 4; 1 | 1 (107968); 4 (27865); 3 (25568) |
| `AFFECTED_LINE_BREAK` | `String` |  | 100.00 | 1 |  |  |
| `AFFECTED_LINE_BREAKING_PASSING_OPTION_ATTEMPTED` | `String` |  | 100.00 | 1 |  |  |
| `AFFECTED_LINE_BREAKING_PASSING_OPTION_DANGEROUS` | `String` |  | 100.00 | 1 |  |  |
| `AFFECTED_LINE_BREAKING_PASSING_OPTION_RUN_SUBTYPE` | `String` |  | 100.00 | 1 |  |  |
| `AFFECTED_LINE_BREAKING_PASSING_OPTION_XTHREAT` | `String` |  | 100.00 | 1 |  |  |
| `ANGLE_OF_ENGAGEMENT` | `String` |  | 100.00 | 1 |  |  |
| `ATTACKING_SIDE` | `String` | Attacking direction label (e.g., left_to_right/right_to_left) | 0.00 | 2 | right_to_left; left_to_right | right_to_left (93229); left_to_right (92480) |
| `BEATEN_BY_MOVEMENT` | `String` |  | 100.00 | 1 |  |  |
| `BEATEN_BY_POSSESSION` | `String` |  | 100.00 | 1 |  |  |
| `BREAK_DEFENSIVE_LINE` | `Boolean` |  | 91.55 | 3 | True; False |  |
| `CARRY` | `String` |  | 100.00 | 1 |  |  |
| `CONSECUTIVE_ON_BALL_ENGAGEMENTS` | `String` |  | 100.00 | 1 |  |  |
| `CURRENT_TEAM_IN_POSSESSION_NEXT_PHASE_TYPE` | `String` |  | 100.00 | 1 |  |  |
| `CURRENT_TEAM_IN_POSSESSION_PREVIOUS_PHASE_TYPE` | `String` |  | 100.00 | 1 |  |  |
| `CURRENT_TEAM_OUT_OF_POSSESSION_NEXT_PHASE_TYPE` | `String` |  | 100.00 | 1 |  |  |
| `CURRENT_TEAM_OUT_OF_POSSESSION_PREVIOUS_PHASE_TYPE` | `String` |  | 100.00 | 1 |  |  |
| `DANGEROUS` | `Boolean` |  | 0.00 | 2 | False; True |  |
| `DEFENSIVE_STRUCTURE` | `String` |  | 100.00 | 1 |  |  |
| `DELTA_TO_LAST_DEFENSIVE_LINE_GAIN` | `Float64` | Depth/offset relative to last defensive line (see task.md for formula checks) | 0.00 | 4478 | 2.96; -11.37 |  |
| `DIFFICULT_PASS_TARGET` | `Boolean` |  | 0.00 | 2 | False; True |  |
| `DISTANCE_COVERED` | `Float64` | Distance covered during event (meters) | 0.00 | 5865 | 5.91; 16.03 |  |
| `DURATION` | `Float64` | Event duration in seconds | 0.00 | 127 | 1.3; 3.1 |  |
| `END_TYPE` | `String` |  | 100.00 | 1 |  |  |
| `FIRST_LINE_BREAK` | `String` |  | 100.00 | 1 |  |  |
| `FIRST_LINE_BREAK_TYPE` | `String` |  | 100.00 | 1 |  |  |
| `FIRST_PLAYER_POSSESSION_IN_TEAM_POSSESSION` | `String` |  | 100.00 | 1 |  |  |
| `FIXTURE_DESCRIPTION` | `String` | Human fixture label (competition/season/teams) | 0.00 | 379 | ENG - Premier League - 2024/2025 Manchester U - Fulham; ENG - Premier League - 2024/2025 Ipswich Town - Liverpool | ENG - Premier League - 2024/2025 Nottingham - Crystal Palace (1039); ENG - Premier League - 2024/2025 Nottingham - West Ham (1018); ENG - Premier League - 2024/2025 Nottingham - Fulham (948) |
| `FORCE_BACKWARD` | `String` |  | 100.00 | 1 |  |  |
| `FORWARD_MOMENTUM` | `String` |  | 100.00 | 1 |  |  |
| `FULLY_EXTRAPOLATED` | `String` |  | 100.00 | 1 |  |  |
| `FURTHEST_LINE_BREAK` | `String` |  | 100.00 | 1 |  |  |
| `FURTHEST_LINE_BREAK_TYPE` | `String` |  | 100.00 | 1 |  |  |
| `GAME_INTERRUPTION_AFTER` | `String` |  | 100.00 | 1 |  |  |
| `GAME_INTERRUPTION_BEFORE` | `String` |  | 100.00 | 1 |  |  |
| `GAME_STATE` | `String` |  | 0.00 | 3 | drawing; losing | drawing (82549); losing (55239); winning (47921) |
| `GIVE_AND_GO` | `Boolean` |  | 0.00 | 2 | False; True |  |
| `HAND_PASS` | `String` |  | 100.00 | 1 |  |  |
| `HIGH_PASS` | `String` |  | 100.00 | 1 |  |  |
| `INDEX` | `Int64` | Event index within match (1-based in this file) | 0.00 | 5667 | 1; 15 |  |
| `INDEX_IN_PRESSING_CHAIN` | `String` |  | 100.00 | 1 |  |  |
| `INITIATE_GIVE_AND_GO` | `String` |  | 100.00 | 1 |  |  |
| `INTENDED_RUN_BEHIND` | `Boolean` |  | 91.55 | 3 | False; True |  |
| `INTERPLAYER_ANGLE` | `String` |  | 100.00 | 1 |  |  |
| `INTERPLAYER_DIRECTION` | `String` |  | 100.00 | 1 |  |  |
| `INTERPLAYER_DISTANCE` | `String` |  | 100.00 | 1 |  |  |
| `INTERPLAYER_DISTANCE_MIN` | `String` |  | 100.00 | 1 |  |  |
| `INTERPLAYER_DISTANCE_RANGE` | `String` |  | 100.00 | 1 |  |  |
| `INTERPLAYER_DISTANCE_START_PHYSICAL` | `String` |  | 100.00 | 1 |  |  |
| `IN_TO_OUT` | `Boolean` |  | 0.00 | 2 | False; True |  |
| `ISSUED_FROM_DIFFERENT_PHASE` | `String` |  | 100.00 | 1 |  |  |
| `IS_HEADER` | `String` |  | 100.00 | 1 |  |  |
| `IS_PASS_RECEPTION_MATCHED` | `Boolean` |  | 72.59 | 3 | True; False |  |
| `IS_PLAYER_POSSESSION_END_MATCHED` | `Boolean` |  | 0.00 | 2 | True; False |  |
| `IS_PLAYER_POSSESSION_START_MATCHED` | `Boolean` |  | 0.00 | 2 | True; False |  |
| `IS_PREVIOUS_PASS_MATCHED` | `String` |  | 100.00 | 1 |  |  |
| `LAST_DEFENSIVE_LINE_HEIGHT_GAIN` | `Float64` |  | 0.00 | 6602 | 0.28; -4.92 |  |
| `LAST_LINE_BREAK` | `String` |  | 100.00 | 1 |  |  |
| `LAST_LINE_BREAK_TYPE` | `String` |  | 100.00 | 1 |  |  |
| `LAST_PLAYER_POSSESSION_IN_TEAM_POSSESSION` | `String` |  | 100.00 | 1 |  |  |
| `LEAD_TO_DIFFERENT_PHASE` | `String` |  | 100.00 | 1 |  |  |
| `LEAD_TO_GOAL` | `Boolean` |  | 0.00 | 2 | False; True |  |
| `LEAD_TO_SHOT` | `Boolean` |  | 0.00 | 2 | False; True |  |
| `N_DEFENSIVE_LINES` | `String` |  | 100.00 | 1 |  |  |
| `N_OFF_BALL_RUNS` | `String` |  | 100.00 | 1 |  |  |
| `N_OPPONENTS_AHEAD_PASS_RECEPTION` | `String` |  | 100.00 | 1 |  |  |
| `N_OPPONENTS_AHEAD_PLAYER_IN_POSSESSION_PASS_MOMENT` | `String` |  | 100.00 | 1 |  |  |
| `N_OPPONENTS_BYPASSED` | `String` |  | 100.00 | 1 |  |  |
| `N_OPPONENTS_OVERTAKEN` | `Int64` |  | 0.00 | 21 | 1; 2 |  |
| `N_PASSING_OPTIONS` | `String` |  | 100.00 | 1 |  |  |
| `N_PASSING_OPTIONS_AHEAD` | `String` |  | 100.00 | 1 |  |  |
| `N_PASSING_OPTIONS_DANGEROUS_DIFFICULT` | `String` |  | 100.00 | 1 |  |  |
| `N_PASSING_OPTIONS_DANGEROUS_NOT_DIFFICULT` | `String` |  | 100.00 | 1 |  |  |
| `N_PASSING_OPTIONS_FIRST_LINE_BREAK` | `String` |  | 100.00 | 1 |  |  |
| `N_PASSING_OPTIONS_LAST_LINE_BREAK` | `String` |  | 100.00 | 1 |  |  |
| `N_PASSING_OPTIONS_LINE_BREAK` | `String` |  | 100.00 | 1 |  |  |
| `N_PASSING_OPTIONS_NOT_DANGEROUS_DIFFICULT` | `String` |  | 100.00 | 1 |  |  |
| `N_PASSING_OPTIONS_NOT_DANGEROUS_NOT_DIFFICULT` | `String` |  | 100.00 | 1 |  |  |
| `N_PASSING_OPTIONS_SECOND_LAST_LINE_BREAK` | `String` |  | 100.00 | 1 |  |  |
| `N_PLAYER_POSSESSIONS_IN_PHASE` | `String` |  | 100.00 | 1 |  |  |
| `N_SIMULTANEOUS_PASSING_OPTIONS` | `String` |  | 100.00 | 1 |  |  |
| `N_SIMULTANEOUS_RUNS` | `Int64` |  | 0.00 | 7 | 0; 1 |  |
| `ONE_TOUCH` | `String` |  | 100.00 | 1 |  |  |
| `OPPONENT_TEAM_SCORE` | `Int64` |  | 0.00 | 8 | 0; 1 |  |
| `ORGANISED_DEFENSE` | `String` |  | 100.00 | 1 |  |  |
| `OUT_TO_IN` | `Boolean` |  | 0.00 | 2 | False; True |  |
| `PASSING_OPTION_AT_PASS_MOMENT` | `String` |  | 100.00 | 1 |  |  |
| `PASSING_OPTION_SCORE` | `Float64` |  | 0.00 | 6305 | 0.7928; 0.939 |  |
| `PASS_AHEAD` | `String` |  | 100.00 | 1 |  |  |
| `PASS_ANGLE` | `String` |  | 100.00 | 1 |  |  |
| `PASS_ANGLE_RECEIVED` | `String` |  | 100.00 | 1 |  |  |
| `PASS_DIRECTION` | `String` |  | 100.00 | 1 |  |  |
| `PASS_DIRECTION_RECEIVED` | `String` |  | 100.00 | 1 |  |  |
| `PASS_DISTANCE` | `String` |  | 100.00 | 1 |  |  |
| `PASS_DISTANCE_RECEIVED` | `String` |  | 100.00 | 1 |  |  |
| `PASS_OUTCOME` | `String` |  | 100.00 | 1 |  |  |
| `PASS_RANGE` | `String` |  | 100.00 | 1 |  |  |
| `PASS_RANGE_RECEIVED` | `String` |  | 100.00 | 1 |  |  |
| `PEAK_PASSING_OPTION_FRAME` | `String` |  | 100.00 | 1 |  |  |
| `PHASE_INDEX` | `Int64` |  | 0.00 | 511 | 0; 1 |  |
| `POSSESSION_DANGER` | `String` |  | 100.00 | 1 |  |  |
| `PREDICTED_PASSING_OPTION` | `Boolean` |  | 0.00 | 2 | True; False |  |
| `PRESSING_CHAIN` | `String` |  | 100.00 | 1 |  |  |
| `PRESSING_CHAIN_END_TYPE` | `String` |  | 100.00 | 1 |  |  |
| `PRESSING_CHAIN_INDEX` | `String` |  | 100.00 | 1 |  |  |
| `PRESSING_CHAIN_LENGTH` | `String` |  | 100.00 | 1 |  |  |
| `PUSH_DEFENSIVE_LINE` | `Boolean` |  | 91.55 | 3 | False; True |  |
| `QUICK_PASS` | `String` |  | 100.00 | 1 |  |  |
| `RECEIVED` | `Boolean` |  | 0.00 | 2 | False; True |  |
| `RECEIVED_IN_SPACE` | `Boolean` |  | 72.57 | 3 | True; False |  |
| `REDUCE_POSSESSION_DANGER` | `String` |  | 100.00 | 1 |  |  |
| `SECOND_LAST_LINE_BREAK` | `String` |  | 100.00 | 1 |  |  |
| `SECOND_LAST_LINE_BREAK_TYPE` | `String` |  | 100.00 | 1 |  |  |
| `SEPARATION_GAIN` | `Float64` | Separation change (end-start; meters) | 0.00 | 2955 | 0.37; 0.25 |  |
| `SIMULTANEOUS_DEFENSIVE_ENGAGEMENT_SAME_TARGET` | `String` |  | 100.00 | 1 |  |  |
| `SIMULTANEOUS_DEFENSIVE_ENGAGEMENT_SAME_TARGET_RANK` | `String` |  | 100.00 | 1 |  |  |
| `SPEED_AVG` | `Float64` | Average speed over event (km/h in this dataset) | 2.07 | 1727 | 17.3; 19.0 |  |
| `SPEED_AVG_BAND` | `String` |  | 2.07 | 4 | running; hsr | running (131062); hsr (42598); sprinting (8204) |
| `SPEED_DIFFERENCE` | `String` |  | 100.00 | 1 |  |  |
| `START_TYPE` | `String` |  | 100.00 | 1 |  |  |
| `STOP_POSSESSION_DANGER` | `String` |  | 100.00 | 1 |  |  |
| `TARGETED` | `Boolean` |  | 0.00 | 2 | False; True |  |
| `TRAJECTORY_ANGLE` | `Float64` | Run trajectory angle (degrees) | 0.00 | 34109 | -120.87; 5.71 |  |
| `TRAJECTORY_DIRECTION` | `String` | Run trajectory direction category | 0.00 | 4 | sideway_right; forward | forward (107968); sideway_right (27865); sideway_left (25568) |
| `XLOSS_PLAYER_POSSESSION_MAX` | `String` |  | 100.00 | 1 |  |  |
| `XPASS_COMPLETION` | `Float64` |  | 0.00 | 8628 | 0.9808; 0.9211 |  |
| `XSHOT_PLAYER_POSSESSION_MAX` | `String` |  | 100.00 | 1 |  |  |
| `XTHREAT` | `Float64` |  | 0.00 | 3737 | 0.0013; 0.002 |  |

Numeric summary:

| Column | min | p05 | p50 | p95 | max | mean | std |
|---|---:|---:|---:|---:|---:|---:|---:|
| `PERIOD` | 1 | 1 | 1 | 2 | 2 | 1.4932 | 0.5 |
| `ATTACKING_SIDE_ID` | 1 | 1 | 2 | 2 | 2 | 1.502 | 0.5 |
| `GAME_STATE_ID` | 1 | 1 | 2 | 3 | 3 | 2.1865 | 0.8172 |
| `SPEED_AVG_BAND_ID` | 2 | 2 | 2 | 3 | 4 | 2.3245 | 0.5562 |
| `TRAJECTORY_DIRECTION_ID` | 1 | 1 | 1 | 4 | 4 | 1.8564 | 1.1396 |
| `DELTA_TO_LAST_DEFENSIVE_LINE_GAIN` | -48.89 | -12.95 | -1.92 | 6.13 | 32.67 | -2.4864 | 5.8226 |
| `DISTANCE_COVERED` | 2.51 | 3.5 | 10.71 | 34.82 | 97.43 | 13.7153 | 10.2487 |
| `DURATION` | 0.7 | 0.8 | 2.1 | 5.7 | 15.6 | 2.5211 | 1.5643 |
| `INDEX` | 0 | 230 | 2.39e+03 | 4.65e+03 | 5.84e+03 | 2.41e+03 | 1.41e+03 |
| `LAST_DEFENSIVE_LINE_HEIGHT_GAIN` | -72.26 | -24.66 | -3.03 | 5.09 | 39.12 | -5.5051 | 9.2429 |
| `N_OPPONENTS_OVERTAKEN` | -10 | -2 | 0 | 4 | 10 | 0.7925 | 1.8522 |
| `N_SIMULTANEOUS_RUNS` | 0 | 0 | 1 | 3 | 6 | 0.7851 | 0.9129 |
| `OPPONENT_TEAM_SCORE` | 0 | 0 | 0 | 3 | 7 | 0.7035 | 0.9472 |
| `PASSING_OPTION_SCORE` | 0.0026 | 0.6257 | 0.8734 | 0.9821 | 0.9995 | 0.8423 | 0.1236 |
| `PHASE_INDEX` | 0 | 21 | 203 | 398 | 529 | 205.7304 | 120.6447 |
| `SEPARATION_GAIN` | -26.72 | -5.77 | -0.25 | 4.4 | 22.1 | -0.4004 | 3.1645 |
| `SPEED_AVG` | 15 | 15.49 | 18.05 | 24.75 | 36.88 | 18.8065 | 2.9478 |
| `TRAJECTORY_ANGLE` | -179.95 | -145.72 | -0.82 | 146.03 | 180 | -1.438 | 76.1687 |
| `XPASS_COMPLETION` | 0.0156 | 0.3372 | 0.802 | 0.9953 | 0.9998 | 0.7448 | 0.2246 |
| `XTHREAT` | 0 | 0.0002 | 0.0093 | 0.1137 | 0.4664 | 0.0277 | 0.0522 |

