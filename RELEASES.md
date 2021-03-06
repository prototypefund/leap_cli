Version 1.9.0
  - Support for leap_platform 0.9
  - Moved almost all the code to leap_platform.
  - Added support for AWS via `leap vm` command.
  - Added support for Let's Encrypt via `leap cert renew` command.
  - Reduced the number of third party gem dependencies, which made initial
    start of leap_cli much faster.

Version 1.8.0
  - better node inheritence, now supports partials.
  - `leap node init` is now governed by an init script
    that gets rsynced to the server.
  - now only supports debian jessie
  - move most leap_cli code to leap_platform
  - added `leap compile firewall`
  - many bug fixes

Version 1.7.2
  - hot fix to better pin the required version of gem gli

Version 1.7.1
  - added 'leap scp' and 'leap tunnel'
  - support for deploy logging and 'leap history'
  - added --force
  - server certs are now autogenerated as needed
  - better mac, debian, and ruby 2.0 compatibility.
  - many bug fixes

Version 1.6.1
  - add environment pinning, see `leap help env`
  - support both rsa and ecdsa host keys
  - custom puppet modules: drop modules in files/puppet/modules
  - all json macros are now moved to the platform
  - allow "+key" and "-key" json properties for adding and subtracting
    arrays during inheritence
  - bugfix: better CSR creation
  - bugfix: always sort arrays in exported json.
  - bugfix: improved cert updating

Version 1.5.6

- Added try{} macro function that quietly swallows exceptions.
- Added ability to scope tags by environment.
- Added rand_range and base32_secret macros.
- Many ssh fixes
- Made --no-color work better
- Prevent `apt-get upgrade` when update fails.
- Always compile all hiera .yaml files.
- Fixs ntpd fixes

Version 1.5.3

- Better utf8 support
- Prevent invalid host names
- Allow json keys with periods in them
- Better `leap new`

Version 1.5.0

- Added ability to scope provider.json by environment

Version 1.2.5

- Added initial support for remote tests.
- Will now bail if /etc/leap/no-deploy is present on a node.
