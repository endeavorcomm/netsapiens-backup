# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added

- logic to ensure older backups are only removed if the `storage` option is set to `"local"`

### Changed

- removal of backups to end of script, to ensure it is only run once a new backup is done

### Fixed

- `keepdays` logic calculation, so it actually keeps this number of days

## [0.3.0-beta] - 2022-01-24

### Added

- logic to remove backups older than N days
- `keepdays` to config file, to support removing older backups. default is `7`
- option to backup audio files from /usr/local/NetSapiens/SiPbx/data, `audiofiles`
- change directory command to script, to ensure the config file is sourced properly

### Changed

- script and config files bash reference from `!/bin/bash` to `!/usr/bin/env bash`

### Fixed

- comparison statements, to use `==` instead of `=`

## [0.2.0-beta] - 2022-01-04

### Added

- this CHANGELOG
- local server directory backup option

## [0.1.2-beta] - 2020-01-10

### Added

Everything by [Sean Cheesman](https://github.com/scheesman)

[Unreleased]: https://github.com/endeavorcomm/netsapiens-backup/compare/v0.3.0-beta...HEAD
[0.3.0-beta]: https://github.com/endeavorcomm/netsapiens-backup/compare/v0.2.0-beta...v0.3.0-beta
[0.2.0-beta]: https://github.com/endeavorcomm/netsapiens-backup/compare/v0.1.2-beta...v0.2.0-beta
[0.1.2-beta]: https://github.com/endeavorcomm/netsapiens-backup/releases/tag/v0.1.2-beta