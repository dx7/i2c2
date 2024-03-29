# DEPRECATED

This fork had been created to fix some bugs. However the original
project has been rewrite to use the new Python API of iTerm2. It 
doesn't rely on any weird AppleScript and should be a bit faster
in generating the ssh windows. Then you should use the original
[i2cssh](https://github.com/wouterdebie/i2cssh) again.


# i2c2

i2c2 is a csshX (http://code.google.com/p/csshx/) like tool for connecting over ssh to multiple machines. But instead of creating separate windows and having
a master window for input, i2c2 uses iterm2 split panes and "Send input to all sessions" (cmd-shift-i) to send commands to all sessions.

i2c2 is based on i2cssh which was a great project created by Wouter de Bie but unfortunately i2cssh had its latest release on July 15, 2016, almost 3 years ago.
i2c2 is here just to keep i2cssh alive and up to date.

## Install

Just run:

    $ gem install i2c2

When using iTerm2 < 2.9, install old i2cssh version 1.16.0:

    $ gem install i2cssh -v 1.16.0

## Migrate from i2cssh

If you have used i2cssh and want to migrate, you can do it painless using i2c2 version 0.99.0 which is 100% compatible with i2cssh 2.2.0 (latest release),
including the config file ~/.i2csshrc.

Version 0.99.1 is compatible with i2cssh branch master at the time of fork (commit: ac0bf90), including auto complete config file name i2cssh-autocomplete.bash.

## Usage
    Usage: i2c2 [options] [(username@host [username@host] | username@cluster)]
    -c, --clusters clus1,clus2       Comma-separated list of clusters specified in ~/.i2csshrc
    -m, --machines a,b,c             Comma-separated list of hosts
    -f, --file FILE                  Cluster file (one hostname per line)
    -t, --tab-split                  Split servers/clusters into tabs (group arguments)
    -T, --tab-split-nogroup          Split servers/clusters into tabs (don't group arguments)
    -A, --forward-agent              Enable SSH agent forwarding
    -l, --login LOGIN                SSH login name
    -e, --environment KEY=VAL        Send environment vars (comma-separated list, need to start with LC_)
    -r, --rank                       Send LC_RANK with the host number as environment variable
    -F, --fullscreen                 Make the window fullscreen
    -C, --columns COLUMNS            Number of columns (rows will be calculated)
    -R, --rows ROWS                  Number of rows (columns will be calculated)
    -b, --broadcast                  Start with broadcast input (DANGEROUS!)
    -nb, --nobroadcast               Disable broadcast
    -p, --profile PROFILE            Name of the iTerm2 profile (default: Default)
    -2, --iterm2                     Use iTerm2 instead of iTerm
    -i, --itermname NAME             Name of the application to use (default: iTerm)
    -s, --sleep SLEEP                Number of seconds to sleep between creating SSH sessions
    -d, --direction DIRECTION        Direction that new sessions are created (default: column)
    -X, --extra EXTRA_PARAM          Additional ssh parameters (e.g. -Xi=myidentity.pem)

For `-c` and `-m` options, the format `username@cluster` or `username@host` can be used.

Using the `-l` option will override all usernames:

    $ i2c2 -l foo user1@host1 user2@host2

This will connect to both `host1` and `host2` as the user `foo`

## i2csshrc

The `i2csshrc` file is a YAML formatted file that contains the following structure:

    ---
    version: 2
    [optional parameters]
    clusters:
      mycluster:
        [optional parameters]
        hosts:
          - host1
          - host2

## Optional Parameters

They can be used globally or per cluster and include:

    broadcast: (true/false)     # Enable/disable broadcast on start
    login: <username>           # Use this username for login
    profile: <iTerm2 profile>   # Use this iTerm profile
    rank: (true/false)          # Enable sending LC_RANK as an environment variable
    columns: <cols>             # Amount of columns
    rows: <rows>                # Amount of rows
    sleep: <secs>               # Seconds to sleep between creating SSH sessions
    direction: (column/row)     # Direction that new sessions are created (default: column)
    itermname:                  # iTerm app name (default: iTerm)

    environment:                # Send the following enviroment variables
        - LC_FOO: foo
        - LC_BAR: bar

    iterm2: true                # Use iTerm2.app instead of iTerm.app (only available globally)

Note: rows and columns can't be used together.

The following precedence is used:

`global options from config` < `cluster options from config` < `command line flags`

Make sure the config file is valid YAML (e.g. use spaces instead of tabs)

## Autocomplete

To allow autocomplete, add the following to your .bash_profile, .bashrc or .profile

    $ source [path to ./extras/i2cssh-autocomplete.bash]

## Options

### -A, --forward-agent

Enable SSH agent forwarding

### -l, --login LOGIN

This option will override all logins passed in to i2c2. This goes for global config, cluster config or username@host passed on the command line

### -e, --environment KEY=VAL

Allows for passing environment varables to the SSH session. This can be a comma-separated list: `-e LC_FOO=foo,LC_BAR=bar`

### -F, --fullscreen

Enable fullscreen on startup

### -C, --columns COLUMNS

Set the amount of columns. Can't be used in conjunction with -R

### -R, --rows ROWS

Set the amount of columns. Can't be used in conjunction with -C

### -b, --broadcast

Enable broadcast on startup. i2c2 will send cmd-shift-i to the window and press the OK button.

### -nb, --nobroadcast

Disable broadcast. This setting can be used to disable any broadcast that was set in the config.

### -p, --profile PROFILE

Use a specific iTerm profile

### -2, --iterm2

Use iTerm2.app instead of iTerm.app

### -i, --itermname NAME

Name of the application to use (default: iTerm). It happens sometimes iTerm isn't called iTerm. Use this parameter to override what app i2c2 interacts with.

### -f, --file

Will read nodes from a file. These will be added to any hosts specified on the command line or in the config

### -c, --clusters clus1,clus2

Connect to one or more clusters that are specified in the config

### -r, --rank

Send a LC_RANK environment variable different for each host (from 0 to n)

### -m, --machines a,b,c

Connect to the machines a, b and c

### -t, --tab-split

Split servers/clusters into tabs, grouping arguments.
Tabs are created as follows: hosts after a -m option are put in one tab, each cluster is always in its own tab, all the arguments are in one tab.

### -T, --tab-split-nogroup

Split servers/clusters into tabs, *not* grouping arguments.
Tabs are created as follows: hosts after a -m option are put in one tab, each cluster is always in its own tab, each argument is in its own tab.

### -s, --sleep SLEEP

Wait SLEEP seconds between starting each ssh session. This will take decimals as well (0.5 for half a second)

### -X, --extra EXTRA

Set extra ssh parameters in the form -Xk=v. For example:

    i2c2 -Xi=myidentity.pem

will result in

    ssh -i myidentity.pem

Or,

    i2c2 -Xp=2222 -XL=8080:localhost:8080

will result in

    ssh -p 2222 -L 8080:localhost:8080

## TODO

- Functional parity with csshX (as far as possible)
- -X support in config file

## Contributing to i2c2

I know that i2c2 doesn't have all the functionality of csshX, but either let me know what you really need or
fork, hack and create a pull request.

 * Check out the latest master to make sure the feature hasn't been implemented or the bug hasn't been fixed yet
 * Check out the issue tracker to make sure someone already hasn't requested it and/or contributed it
 * Fork the project
 * Start a feature/bugfix branch
 * Commit and push until you are happy with your contribution
 * Make sure to add tests for it. This is important so I don't break it in a future version unintentionally.
 * Please try not to mess with the Rakefile, version, or history. If you want to have your own version, or is otherwise necessary, that is fine, but please isolate to its own commit so I can cherry-pick around it.

## Copyright

See LICENSE.txt for further details.
