# Reference Information

The following sections provide detailed reference information for the various functions you can use in the game.

## Table of Contents
- [Global Query Functions](#global-query-functions)
- [Robot Query Functions](#robot-query-functions)
- [Sensing Functions](#sensing-functions)
- [Readiness Functions](#readiness-functions)
- [Movement Functions](#movement-functions)
- [Attack Functions](#attack-functions)
- [Marking Functions](#marking-functions)
- [Building Functions](#building-functions)
- [Communication Functions](#communication-functions)
- [Transfer Paint Functions](#transfer-paint-functions)
- [Upgrade Tower Functions](#upgrade-tower-functions)
- [Debug Indicator Functions](#debug-indicator-functions)
- [Clock Methods](#clock-methods)

### log

`def log(msg: str) -> None`

Logs a message to the console.

- **Parameters**:
  - `msg` (str): The message to log.
- **Returns**: None
- **Bytecode Cost**: 0

---

## Global Query Functions

### get_round_num

`def get_round_num() -> int`

Returns the current round number, where 1 is the first round of the match.

- **Returns**: The current round number, where 1 is the first round of the match.
- **Bytecode Cost**: 1

### get_map_width

`def get_map_width() -> int`

Returns the width of the map. Valid x coordinates range from 0 (inclusive) to the width (exclusive).

- **Returns**: The width of the map.
- **Bytecode Cost**: 1

### get_map_height

`def get_map_height() -> int`

Returns the height of the game map. Valid y coordinates range from 0 (inclusive) to the height (exclusive).

- **Returns**: The height of the game map.
- **Bytecode Cost**: 1

### get_resource_pattern()

`def get_resource_pattern() -> List[List[bool]]`

Returns the 5x5 resource pattern.

- **Returns**: A boolean array of arrays, where entry [i][j] is true if the i'th row and the j'th column of the pattern should use the secondary color.
- **Bytecode Cost**: 2

### get_tower_pattern

`def get_tower_pattern(tower_type: UnitType) -> List[List[bool]]`

Returns the 5x5 pattern needed to be drawn to build a tower of the specified type.

- **Parameters**:
  - `tower_type`: the type of tower to build. Must be a tower type.
- **Returns**: A boolean array of arrays, where entry [i][j] is true if the i'th row and the j'th column of the pattern should use the secondary color.
- **Bytecode Cost**: 2

### get_num_towers

`def get_num_towers() -> int`

Returns how many allied towers are currently alive.

- **Returns**: The number of alive allied towers.
- **Bytecode Cost**: 5

---

## Robot Query Functions

### get_id

`def get_id() -> int`

Returns the ID of this robot.

- **Returns**: The ID of this robot.
- **Bytecode Cost**: 1

### get_team

`def get_team() -> Team`

Returns this robot's team.

- **Returns**: This robot's team.
- **Bytecode Cost**: 1

### get_paint

`def get_paint() -> int`

Returns this robot's current paint amount.

- **Returns**: This robot's current paint amount.
- **Bytecode Cost**: 1

### get_location

`def get_location() -> MapLocation`

Returns this robot's current location.

- **Returns**: This robot's current location.
- **Bytecode Cost**: 1

### get_health

`def get_health() -> int`

Returns this robot's current health.

- **Returns**: This robot's current health.
- **Bytecode Cost**: 1

### get_money

`def get_money() -> int`

Returns the amount of money that this robot's team has.

- **Returns**: The amount of money that this robot's team has.
- **Bytecode Cost**: 1

### get_chips

`def get_chips() -> int`

Alias for get_money.

- **Returns**: The amount of money that this robot's team has.
- **Bytecode Cost**: 1

### get_type

`def get_type() -> UnitType`

Returns what UnitType this robot is.

- **Returns**: The UnitType of this robot.
- **Bytecode Cost**: 1

---

## Sensing Functions

### on_the_map

`def on_the_map(loc: MapLocation) -> bool`

Checks whether a MapLocation is on the map.

- **Parameters**:
  - `loc`: the MapLocation to check.
- **Returns**: True if the location is on the map, false otherwise.
- **Bytecode Cost**: 5

### can_sense_location

`def can_sense_location(loc: MapLocation) -> bool`

Checks whether the given location is within the robot's vision range, and if it is on the map.

- **Parameters**:
  - `loc`: the location to check.
- **Returns**: True if the location is on the map and within the robot's vision range, false otherwise.
- **Bytecode Cost**: 5

### is_location_occupied

`def is_location_occupied(loc: MapLocation) -> bool`

Checks whether a robot is at a given location. Assumes the location is valid.

- **Parameters**:
  - `loc`: the location to check.
- **Returns**: True if a robot is at the given location, false otherwise.
- **Bytecode Cost**: 5

### can_sense_robot_at_location

`def can_sense_robot_at_location(loc: MapLocation) -> bool`

Checks whether a robot is at a given location. Assumes the location is valid.
- **Parameters**:
  - `loc`: the location to check.
- **Returns**: True if a robot is at the location, false if there is no robot or the location cannot be sensed.

### sense_robot_at_location

`def sense_robot_at_location(loc: MapLocation) -> RobotInfo | None`

Sense the robot at the given location, or None if there is no robot there.

- **Parameters**:
  - `loc`: the location to sense.
- **Returns**: The robot at the given location
- **Bytecode Cost**: 15

### can_sense_robot

`def can_sense_robot(robot_id: int) -> bool`

Tests whether the given robot exists and if it is within this robot's vision range.

- **Parameters**:
  - `robot_id`: the ID of the robot to check.
- **Returns**: True if the robot exists and is within this robot's vision range, false otherwise.
- **Bytecode Cost**: 5

### sense_robot

`def sense_robot(robot_id: int) -> RobotInfo | None`

Sense information about a particular robot given its ID.

- **Parameters**:
  - `robot_id`: the ID of the robot to sense.
- **Returns**: A RobotInfo object for the sensed robot.

### sense_nearby_robots

`def sense_nearby_robots(center: MapLocation = None, radius_squared: int = GameConstants.VISION_RADIUS_SQUARED, team: Team = None) -> List[RobotInfo]`

Returns all robots of a given team that can be sensed within a certain radius of a specified location. The objects are returned in no particular order.

- **Parameters**:
  - `center`: the center of the given search radius. If None, uses this robot's location.
  - `radius_squared`: return robots this distance away from the center of this robot; if -1 is passed, all robots within vision radius are returned; if radius_squared is larger than the robot's vision radius, the vision radius is used
  - `team`: filter game objects by the given team; if None is passed, objects from all teams are returned
- **Returns**: A list of RobotInfo objects for all robots that can be sensed within the given radius of the given location.
- **Bytecode Cost**: 100

### sense_passability

`def sense_passability(loc: MapLocation) -> bool`

Given a senseable location, returns whether that location is passable (a wall).

- **Parameters**:
  - `loc`: the location to check.
- **Returns**: True if the location is passable, false otherwise.
- **Bytecode Cost**: 5

### sense_nearby_ruins

`def sense_nearby_ruins(radius_squared: int = GameConstants.VISION_RADIUS_SQUARED) -> List[MapLocation]`

Returns the location of all nearby ruins that are visible to the robot. If radius_squared is greater than the robot's vision radius, uses the robot's vision radius instead.

- **Parameters**:
  - `radius_squared`: the radius to search for ruins; -1 for max radius
- **Returns**: All locations containing ruins within the given radius.
- **Bytecode Cost**: 100

### sense_map_info

`def sense_map_info(loc: MapLocation) -> MapInfo | None`

Senses the map info at a location. MapInfo includes walls, paint, marks, and ruins

- **Parameters**:
  - `loc`: the location to sense.
- **Returns**: The MapInfo at the given location, or None if the location is not senseable.
- **Bytecode Cost**: 5

### sense_nearby_map_infos

`def sense_nearby_map_infos(center: MapLocation = None, radius_squared: int = GameConstants.VISION_RADIUS_SQUARED) -> List[MapInfo]`

Return map info for all senseable locations within a radius squared of a center location. If radius_squared is larger than the robot's vision radius, uses the robot's vision radius instead. If -1 is passed, all locations within vision radius are returned. MapInfo includes walls, paint, marks, and ruins

- **Parameters**:
  - `center`: the center of the given search radius. If None, uses this robot's location.
  - `radius_squared`: return map info for locations this distance away from the center of this robot; if -1 is passed, all locations within vision radius are returned; if radius_squared is larger than the robot's vision radius, the vision radius is used
- **Returns**: A list of MapInfo objects for all senseable locations within the given radius of the given location.
- **Bytecode Cost**: 100

### get_all_locations_within_radius_squared

`def get_all_locations_within_radius_squared(center: MapLocation, radius_squared: int) -> List[MapLocation]`

Returns a list of all locations within the given radius_squared of a location. If radius_squared is larger than the robot's vision radius, uses the robot's vision radius instead. Checks that radius_squared is non-negative.

- **Parameters**:
  - `center`: the center of the given search radius.
  - `radius_squared`: return locations this distance away from the center of this robot; if radius_squared is larger than the robot's vision radius, the vision radius is used
- **Returns**: A list of all locations within the given radius_squared of the given location.
- **Bytecode Cost**: 100

### adjacent_location

`def adjacent_location(direction: Direction) -> MapLocation`

Returns the location adjacent to current location in the given direction.

- **Parameters**:
  - `direction`: the direction to get the adjacent location in.
- **Returns**: The location adjacent to current location in the given direction.
- **Bytecode Cost**: 1

---

## Readiness Functions

### is_action_ready

`def is_action_ready() -> bool`

Tests whether the robot can act.

- **Returns**: True if the robot can act, false otherwise.
- **Bytecode Cost**: 1

### get_action_cooldown_turns

`def get_action_cooldown_turns() -> int`

Returns the number of action cooldown turns remaining before this unit can act again. When this number is strictly less than GameConstants.COOLDOWN_LIMIT, is_action_ready() is true and the robot can act again. This number decreases by GameConstants.COOLDOWNS_PER_TURN every turn.

- **Returns**: The number of action cooldown turns remaining before this unit can act again.
- **Bytecode Cost**: 1

### is_movement_ready

`def is_movement_ready() -> bool`

Tests whether the robot can move.

- **Returns**: True if the robot can move, false otherwise.
- **Bytecode Cost**: 1

### get_movement_cooldown_turns

`def get_movement_cooldown_turns() -> int`

Returns the number of movement cooldown turns remaining before this unit can move again. When this number is strictly less than GameConstants.COOLDOWN_LIMIT, is_movement_ready() is true and the robot can move again. This number decreases by GameConstants.COOLDOWNS_PER_TURN every turn.

- **Returns**: The number of movement cooldown turns remaining before this unit can move again.
- **Bytecode Cost**: 1

## Movement Functions

### can_move

`def can_move(direction: Direction) -> None`

Checks whether this robot can move one step in the given direction. Returns false if the robot is not in a mode that can move, if the target location is not on the map, if the target location is occupied, if the target location is impassible, or if there are cooldown turns remaining.

- **Parameters**:
  - `direction`: the direction to check.
- **Returns**: True if the robot can move in the given direction, false otherwise.
- **Bytecode Cost**: 10

### move

`def move(direction: Direction) -> None`

Moves one step in the given direction.

- **Parameters**:
  - `direction`: the direction to move.
- **Returns**: None
- **Bytecode Cost**: 0

---

## Attack Functions

### can_attack

`def can_attack(loc: MapLocation) -> bool`


### attack

`def attack(loc: MapLocation, use_secondary_color: bool = False) -> None`


### can_mop_swing

`def can_mop_swing(dir: Direction) -> bool`

### mop_swing

`def mop_swing(dir: Direction) -> None`


### can_paint

`def can_paint(loc: MapLocation) -> bool`

---

## Marking Functions

### can_mark_tower_pattern

`def can_mark_tower_pattern(tower_type: UnitType, loc: MapLocation) -> bool`

### can_mark_resource_pattern

`def can_mark_resource_pattern(loc: MapLocation) -> bool`

### mark_tower_pattern

`def mark_tower_pattern(tower_type: UnitType, loc: MapLocation) -> None`

### mark_resource_pattern

`def mark_resource_pattern(loc: MapLocation) -> None`

### can_mark

`def can_mark(loc: MapLocation) -> bool`

### mark

`def mark(loc: MapLocation, use_secondary_color: bool) -> None`

### can_remove_mark

`def can_remove_mark(loc: MapLocation) -> bool`

### remove_mark

`def remove_mark(loc: MapLocation) -> None`

### can_complete_tower_pattern

`def can_complete_tower_pattern(tower_type: UnitType, loc: MapLocation) -> bool`

### complete_tower_pattern

`def complete_tower_pattern(tower_type: UnitType, loc: MapLocation) -> None`

### complete_resource_pattern

`def complete_resource_pattern(loc: MapLocation) -> None`

---

## Building Functions

### can_build_robot

`def can_build_robot(robot_type: UnitType, map_location: MapLocation) -> bool`

Checks if a tower can spawn a robot at the given location. Robots can spawn within a circle of radius of sqrt(4) of the tower.

- **Parameters**:
  - `robot_type`: the type of robot to build. Must be a robot type.
  - `map_location`: the location to build the robot at.
- **Returns**: True if the tower can spawn a robot of the given type at the given location, false otherwise.
- **Bytecode Cost**: 10

### build_robot

`def build_robot(robot_type: UnitType, map_location: MapLocation) -> None`

Spawns a robot at the given location. Robots can spawn within a circle of radius of sqrt(4) of the tower.

- **Parameters**:
  - `robot_type`: the type of robot to build. Must be a robot type.
  - `map_location`: the location to build the robot at.
- **Returns**: None
- **Bytecode Cost**: 20

---

## Communication Functions

### can_send_message

`def can_send_message(loc: MapLocation) -> bool`

### send_message

`def send_message(loc: MapLocation, message_content: int) -> None`

### read_message

`def read_message(round: int = -1) -> List[Message]`

### can_broadcast_message

`def can_broadcast_message() -> bool`

### broadcast_message

`def broadcast_message(message_content: int) -> None`

---

## Transfer Paint Functions

### can_transfer_paint

`def can_transfer_paint(loc: MapLocation, amount: int) -> bool`

### transfer_paint

`def transfer_paint(loc: MapLocation, amount: int) -> None`

---

## Upgrade Tower Functions

### can_upgrade_tower

`def can_upgrade_tower(tower_location: MapLocation) -> bool`

### upgrade_tower

`def upgrade_tower(tower_location: MapLocation) -> None`

---

## Debug Indicator Functions

### set_indicator_string

`def set_indicator_string(string: str) -> None`

### set_indicator_dot

`def set_indicator_dot(loc: MapLocation, red: int, green: int, blue: int) -> None`

### set_indicator_line

`def set_indicator_line(start: MapLocation, end: MapLocation, red: int, green: int, blue: int) -> None`

### set_timeline_marker

`def set_timeline_marker(label: str, red: int, green: int, blue: int) -> None`

### resign

`def resign() -> None`

### disintegrate

`def disintegrate() -> None`

---

## Clock Methods

### yield_turn

`def yield_turn() -> None`

### get_bytecode_num

`def get_bytecode_num() -> int`

### get_bytecode_left

`def get_bytecode_left() -> int`

### get_time_elapsed

`def get_time_elapsed() -> int`

### get_time_left

`def get_time_left() -> int`

---