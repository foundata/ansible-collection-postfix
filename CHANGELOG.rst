=================================================
foundata.postfix Ansible collection Release Notes
=================================================

.. contents:: Topics

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
