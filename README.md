# `mkdirs` Role

This role handles making directories. It's basically a directory-specific
wrapper around `file` that you can use in playbooks to get directories
created at the time you need them. (You can, of course, get directories
created by just using `file` yourself in the `tasks` section of the
playbook, but all of that happens after all your roles, and you might need
a directory to exist before a role gets run. That's what this role is
for.)

You can execute this role multiple times by just passing different lists
of directories to the `directories` parameter. You could put those lists
directly into the role invocation, as in the Example Playbook section
below, but it's probably better if you set variables to pass in instead.

Role Variables
--------------

    directories: []

A list of dictionaries describing the directories that need to be created
and the attributes to set to them. The keys of the dictionaries are
identical to the parameters of the `file` module, but not all of them are
represented. The ones that do work are:

*   `path`: **Required.** The path of the directory name to create. The
    list of directories will be sorted on this field to ensure the best
    chance that higher level directories are created below lower level
    ones, assuming the higher level directories are in the list somewhere.
    (This sorting is reversed if `state` is set to `absent`; see below for
    more on that key.)

*   `owner`, `group`, `mode`: **Optional.** All pretty much what they
    sound like; see the `file` module for more.

*   `state`: **Optional.** *Ignored* unless set to `absent`, in which case
    the directory will be removed. No attempts will be made to ensure that
    the directory is empty before trying to remove it.

Example Playbook
----------------

    - hosts: servers
      roles:
         - role: slaarti.mkdirs
           directories:
           - path: "/etc/example"
             owner: root
             group: root
             mode: 0775
           - path: "/etc/example/deeper"
             owner: root
             group: www-data
             mode: 0575

License
-------

Unlicense; see the LICENSE file.

Author Information
------------------

Chris Pinard <slaarti@gmail.com>
