# Changelog

All notable changes to this project will be documented in this file.

## [0.2.2] - Various Fixes

### Fixed

- Issue [14](https://github.com/theelims/PsychicMqttClient/issues/14) was fixed.
- Issue [13](https://github.com/theelims/PsychicMqttClient/issues/13) was fixed.

### Added

- The function `forceStop()` was added to address issue [12](https://github.com/theelims/PsychicMqttClient/issues/12).

## [0.2.1] - Fixing Wildcard Behavior

### Changed

- Issue [6](https://github.com/theelims/PsychicMqttClient/issues/6) was fixed.

### Added

- Example for wildcards in topics.

## [0.2.0] - Compatible with Arduino 3 / ESP-IDF 5

### Added

- CI workflow
- ESP-IDF 5 compatibility

### Changed

- updated library.json

## [0.1.1] - Stability Fixes

### Fixed

- Fixed subscribe error when not connected
- Freezing of event-loop when disconnect() is called while the MQTT server is not connected

## [0.1.0] - Initial Release
