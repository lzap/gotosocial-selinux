# GoToSocial SELinux poclicy

SELinux policies for GoToSocial

## Assumptions

The policy assumes that:

* `gotosocial` binary is either in `/usr/bin` or `/usr/local/bin`.
* `config.yaml` binary is either in `/etc/gotosocial` or `/usr/local/etc/gotosocial`.
* Data is either in `/var/lib/gotosocial` or `/gotosocial`.
* Postgres is running locally or remotely on 5432 port.

## Compiling and loading

To locally compile and load the policy:

    make -f /usr/share/selinux/devel/Makefile load
    /usr/sbin/semodule -i gotosocial.pp
    ./gotosocial-selinux-relabel

Then restart the service

## Changing PostgreSQL port

    semanage port -a -t postgresql_port_t -p tcp 5454

## Changing listening HTTP port

Run this command (no restart of anything is needed):

    semanage port -a -t http_port_t -p tcp 8085

## Debugging CIL

From time to time, SELinux spills out a cryptic error. To generate CLI source from te/pp file, do:

    cat foreman.pp | /usr/libexec/selinux/hll/pp > /tmp/foreman.cil

## License

Copyright (c) 2022 Lukas Zapletal

This program and entire repository is free software: you can redistribute it
and/or modify it under the terms of the GNU General Public License as
published by the Free Software Foundation, either version 3 of the License, or
any later version.

This program is distributed in the hope that it will be useful, but WITHOUT
ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS
FOR A PARTICULAR PURPOSE.  See the GNU General Public License for more
details.

You should have received a copy of the GNU General Public License along with
this program.  If not, see <http://www.gnu.org/licenses/>.
