=================================================
foundata.postfix Ansible collection Release Notes
=================================================

.. contents:: Topics

v1.3.0
======

Release Summary
---------------

Release Date: 2026-05-10

Maintenance and bugfix release.

Minor Changes
-------------

- Added Fedora 44 as supported platform for all collection roles and Molecule test scenarios.
- Added Ubuntu 26.04 LTS (Resolute Raccoon) as supported platform for all collection roles and Molecule test scenarios.

Removed Features (previously deprecated)
----------------------------------------

- Removed Fedora 42 support (End of Life, EOL) from collection roles and Molecule scenarios. The collection may still work on Fedora 42, but no testing or bugfixes will be provided. A warning will be displayed when used on unsupported platforms.

Bugfixes
--------

- ``foundata.postfix.run`` - Fixed uninstallation failure on Fedora 43/44 (and potentially other platforms in the future) caused by DNF dependency resolution errors. Map backend packages (e.g. ``postfix-pcre``) installed during setup were not included in the removal list, preventing the base ``postfix`` package from being removed. All map backend packages are now included in ``__run_postfix_packages_uninstall`` and removed together with the base package.

v1.2.0
======

Release Summary
---------------

Release Date: 2025-12-26

Maintenance release.

Minor Changes
-------------

- Added Fedora 43 as supported platform for all collection roles and Molecule test scenarios

Removed Features (previously deprecated)
----------------------------------------

- Removed Fedora 41 support (End of Life, EOL) from collection roles and Molecule scenarios. The collection may still work on Fedora 41, but no testing or bugfixes will be provided. A warning will be displayed when used on unsupported platforms.

v1.1.0
======

Release Summary
---------------

Release Date: 2025-11-02

Maintenance release.

Minor Changes
-------------

- Molecule: Added Debian 13 (Trixie) as a test target platform.
- Prevent duplicate alias declarations by commenting out previously existing aliases in the lookup-table when they are also defined in ``run_postfix_aliases_map`` or ``__run_postfix_aliases_map_defaults``.
- ``foundata.postfix.run``: Added Debian 13 (Trixie) as a supported platform.

Removed Features (previously deprecated)
----------------------------------------

- Molecule: Removed Debian 11 (Bullseye) as a test target platform.
- ``foundata.postfix.run``: Removed Debian 11 (Bullseye) from the list of supported platforms. The role will continue to work on Debian 11 but will display a warning. To avoid this, either remain on or pin the previous version of the collection. Bugs and issues related to Debian 11 will no longer be fixed.

v1.0.0
======

Release Summary
---------------

Release Date: 2025-05-30

First public release, providing all functionality and files.
