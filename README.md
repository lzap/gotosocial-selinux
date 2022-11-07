# GoToSocial SELinux poclicy

SELinux policicy for
[GoToSocial](https://github.com/superseriousbusiness/gotosocial), an
ActivityPub social network server, written in Golang.

## Assumptions

The policy assumes that:

* `gotosocial` binary is either in `/usr/bin` or `/usr/local/bin`.
* `config.yaml` binary is either in `/etc/gotosocial` or `/usr/local/etc/gotosocial`.
* Data is either in `/var/lib/gotosocial` or `/gotosocial`.
* Postgres is running locally or remotely on 5432 port.

## Compiling and loading

To locally compile and load the policy:

    make -f /usr/share/selinux/devel/Makefile load

Files need to be relabelled, you only need to do this once.

    ./gotosocial-selinux-relabel

Then restart the gotosocial service. You are all set!

## Changing PostgreSQL port

    semanage port -a -t postgresql_port_t -p tcp 5454

## Changing listening HTTP port

Run this command (no restart of anything is needed):

    semanage port -a -t http_port_t -p tcp 8085

## Troubleshooting

When you encounter errors, and you will as the software will get more features, do this:

    sepolgen-ifgen
    audit2allow -larR

Open an issue wiwh output from the `audit2allow` command.

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
