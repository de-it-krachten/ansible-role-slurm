# [1.10.0](https://github.com/de-it-krachten/ansible-role-slurm/compare/v1.9.1...v1.10.0) (2025-10-12)


### Features

* Drop support for SLES/OpenSuse ([bb55f08](https://github.com/de-it-krachten/ansible-role-slurm/commit/bb55f0894af89ab16320f3638806b28d680527bd))
* Introduction of boolean 'slurm_install_drmaa' ([686eaef](https://github.com/de-it-krachten/ansible-role-slurm/commit/686eaefe67c8aec8736d2faa93f1ae5cd7829f03))
* Support for all-in-one (master/db/node) ([c9805dc](https://github.com/de-it-krachten/ansible-role-slurm/commit/c9805dc2db6c95474ff793067d1a045041b90427))

## [1.9.1](https://github.com/de-it-krachten/ansible-role-slurm/compare/v1.9.0...v1.9.1) (2025-08-11)


### Bug Fixes

* Disable password expiration for slurm user ([e70f047](https://github.com/de-it-krachten/ansible-role-slurm/commit/e70f047f7a1c3b9938fc88b02f3511c3eaea4ab5))

# [1.9.0](https://github.com/de-it-krachten/ansible-role-slurm/compare/v1.8.1...v1.9.0) (2024-12-29)


### Features

* Update supported platforms & CI ([3ff857f](https://github.com/de-it-krachten/ansible-role-slurm/commit/3ff857fe3e9365bd9ade536213926e897a6dcf88))

## [1.8.1](https://github.com/de-it-krachten/ansible-role-slurm/compare/v1.8.0...v1.8.1) (2024-10-18)


### Bug Fixes

* Replace symlinks for defaults/Ubuntu-(18|22).yml to actual files ([a38f8b8](https://github.com/de-it-krachten/ansible-role-slurm/commit/a38f8b89f6057e4ab1206af4c3005fdae176f7cb))

# [1.8.0](https://github.com/de-it-krachten/ansible-role-slurm/compare/v1.7.3...v1.8.0) (2024-06-03)


### Bug Fixes

* SelectType from 'select/cons_res' to 'select/cons_tres' ([5a1586f](https://github.com/de-it-krachten/ansible-role-slurm/commit/5a1586f42276d29a4d4e81b87f204a069b629594))


### Features

* Add support for slurm reboot commands ([6bc8414](https://github.com/de-it-krachten/ansible-role-slurm/commit/6bc8414ff34e4597fb7e071ae61f4a5dea80eda2))
* Add support for Ubuntu 24.04 LTS + Fedora 40 ([f4ebbf2](https://github.com/de-it-krachten/ansible-role-slurm/commit/f4ebbf2a392a858e0089d63fb6240795c20725f6))
* Add support for Ubuntu 2404 LTS ([d356674](https://github.com/de-it-krachten/ansible-role-slurm/commit/d356674f496bceda17b68b3f6ffc30a24cce2266))

## [1.7.3](https://github.com/de-it-krachten/ansible-role-slurm/compare/v1.7.2...v1.7.3) (2024-04-12)


### Bug Fixes

* Fix for slurm.conf jinja rendering ([541b25a](https://github.com/de-it-krachten/ansible-role-slurm/commit/541b25a318e25127b585756a3ba567edc3f23dcf))

## [1.7.2](https://github.com/de-it-krachten/ansible-role-slurm/compare/v1.7.1...v1.7.2) (2024-04-12)


### Bug Fixes

* Empty partitions are no longer created ([5e6b959](https://github.com/de-it-krachten/ansible-role-slurm/commit/5e6b959fb6cd02e51d3c048d2c9bd47c9ac2eff0))
* Update DRMAA packages ([e5c3410](https://github.com/de-it-krachten/ansible-role-slurm/commit/e5c341028138bc83b76e9d66c1d939845fd3ca68))

## [1.7.1](https://github.com/de-it-krachten/ansible-role-slurm/compare/v1.7.0...v1.7.1) (2023-09-22)


### Bug Fixes

* Fix Ubuntu 22.04 LTS compatibility ([43f8f30](https://github.com/de-it-krachten/ansible-role-slurm/commit/43f8f3062bb758791ef81fb28e4ad2d1859d055e))

# [1.7.0](https://github.com/de-it-krachten/ansible-role-slurm/compare/v1.6.0...v1.7.0) (2023-09-17)


### Bug Fixes

* Fix loop label to string ([f3f752f](https://github.com/de-it-krachten/ansible-role-slurm/commit/f3f752fddb46e7c7e28154fc444bb888fcc38eb8))
* Fix Ubuntu 22.04 LTS compatibility ([c275849](https://github.com/de-it-krachten/ansible-role-slurm/commit/c275849f96abb255cfd0f5599b43dfa45278265a))
* Refactor code for SLES/OpenSUSE support ([530d6de](https://github.com/de-it-krachten/ansible-role-slurm/commit/530d6de918513bb2f98f112eb1a0938ef123fef7))
* Remove SlurmDB IP from node config ([f254082](https://github.com/de-it-krachten/ansible-role-slurm/commit/f254082e397463b9f13c74624afe95e05d6955b9))


### Features

* Update supported platforms & CI ([e385d84](https://github.com/de-it-krachten/ansible-role-slurm/commit/e385d845514ac210a5a8a013899adf9624390d87))

# [1.6.0](https://github.com/de-it-krachten/ansible-role-slurm/compare/v1.5.0...v1.6.0) (2023-06-23)


### Features

* Add support for user removal from slurm ([41e080b](https://github.com/de-it-krachten/ansible-role-slurm/commit/41e080b3f7d69688c2b76ba87a3f120c2e8d9383))

# [1.5.0](https://github.com/de-it-krachten/ansible-role-slurm/compare/v1.4.0...v1.5.0) (2023-05-06)


### Bug Fixes

* Change slurm-drmaa -> slurm-drmaa1 ([e263e66](https://github.com/de-it-krachten/ansible-role-slurm/commit/e263e66c72d4173a2f5a9b083dde982f10535331))


### Features

* Move to namespaced role names ([f7cb258](https://github.com/de-it-krachten/ansible-role-slurm/commit/f7cb258b8e3fcf2f32555a64c2a37c017efc265e))

# [1.4.0](https://github.com/de-it-krachten/ansible-role-slurm/compare/v1.3.0...v1.4.0) (2022-10-12)


### Bug Fixes

* Disable CI on Debian11 as it fails ([51590b6](https://github.com/de-it-krachten/ansible-role-slurm/commit/51590b6e2c533ad1948c754563d35fee326f3f31))


### Features

* Move to FQCN ([764669c](https://github.com/de-it-krachten/ansible-role-slurm/commit/764669cf8ce97b29874dbc34091f683b95d38425))
* Update CI to latest standards ([b00ef6f](https://github.com/de-it-krachten/ansible-role-slurm/commit/b00ef6f6142b32f2b1865486e2516820a908a096))

# [1.3.0](https://github.com/de-it-krachten/ansible-role-slurm/compare/v1.2.0...v1.3.0) (2022-07-28)


### Features

* Implement ansible-lint v6 support ([be1988e](https://github.com/de-it-krachten/ansible-role-slurm/commit/be1988ec1bfabfb3fca120c4161b42c312aae3de))

# [1.2.0](https://github.com/de-it-krachten/ansible-role-slurm/compare/v1.1.0...v1.2.0) (2022-07-09)


### Features

* Add support for several new platforms ([862c265](https://github.com/de-it-krachten/ansible-role-slurm/commit/862c265883f6d46850c974f5e862985b22699f33))

# [1.1.0](https://github.com/de-it-krachten/ansible-role-slurm/compare/v1.0.0...v1.1.0) (2022-04-02)


### Features

* add support for jtw token as alternative authentication ([82801ad](https://github.com/de-it-krachten/ansible-role-slurm/commit/82801adfa8670d437a4f828281084ae89b33ef6c))

# 1.0.0 (2022-01-19)


### Features

* First release ([8cf4020](https://github.com/de-it-krachten/ansible-role-slurm/commit/8cf40201a5660bb86e0bbe4457a822af9d6abafb))
