# Battery

![image](http://i.imgur.com/mEEPD.png)

Battery is a little bash script that can use [Spark](https://github.com/holman/spark) to display the battery status on your __tmux__ sessions or the __terminal__.

### Features

* Changes color to reflect battery status (Green, Yellow, Red)
* Displays battery percentage
* Graph bar changes its values between 0 and 100%
* If you don't like the default colors, you can specify the good, medium and warning battery status colors using flags (read usage).

### Requirements

There are no external requirements to use __Battery__; however, there are a number of programs that interface well with __Battery__

[Spark](https://github.com/holman/spark) is used by default, if it is installed and in your path.

If you are using a Linux laptop, __Battery__ may use `upower`, which is included with many Linux distributions.

If you don't want to use Spark, you can use the `-a` flag, for ascii output:
![image](http://i.imgur.com/w9qtQeu.png)

# Install - Mac

### Homebrew

Just do (case sensitive)

    brew tap Goles/battery
    brew install battery

### One Liner
(Cut & Paste on terminal to install on `/usr/bin`, btw, try to run from `~/` or other writable dir)

	brew install spark; curl -O https://raw.github.com/Goles/Battery/master/battery ; \
	sudo mv battery /usr/bin; sudo chmod 755 /usr/bin/battery


### Step by Step

* Install spark (with [Homebrew](https://github.com/mxcl/homebrew) on Mac OS X)

	``` brew install spark```

* Copy battery somewhere in your path & fix permissions

	``` sudo cp battery /usr/bin ```

	``` sudo chmod 755 /usr/bin/battery ```


# Install - Linux

Linux support is still being tested. It is recommended, but not necessary,
to have `upower` installed on your machine.

It is recommended to install this somewhere in your path that is writable,
like `/usr/local/bin`

```bash
# if you also want to use spark
curl -O https://raw.githubusercontent.com/holman/spark/master/spark
mv spark /usr/local/bin
chmod u+x /usr/local/bin/spark

curl -O https://raw.githubusercontent.com/goles/battery/master/battery
mv battery /usr/local/bin
chmod u+x /usr/local/bin/battery
```
__NOTE:__ This `spark` is *not* the same `spark` that you would install by doing

```bash
$ sudo aptitude install spark
```
That is [Apache Spark](http://spark.apache.org), which is a general engine for
large-scale data processing.


# Usage

### Terminal

* Run Battery (From the terminal)

	``` battery ```
###### You should see something like this:
![image](http://i.imgur.com/SLSBg.png)

### tmux

* Be sure to make tmux display utf-8 characters by running it with the `-u` flag

	```tmux -u```

* Add the following line to your `~/.tmux.conf` file

	``` set -g status-right "#(/usr/bin/battery -t)"```

* reload the tmux config by running `tmux source-file ~/.tmux.conf`.

###### You should now see something like this at the bottom right corner:
![image](http://i.imgur.com/Eaajb.png)

### Flags
`-t` if you are using `battery` in a `tmux` status bar.
`-a` for ascii output. `battery` reverts to this setting if `spark` is not found.
`-p` for alternative program. On Mac this uses `pmset`, and on Linux switches to using `upower`, if present. Both give more accurate percentages.
`-b` for alternative battery path (ignored on Mac)

The flag `-b` will set a different battery path, the default value is `/sys/class/power_supply/BAT0`. Use this flag for specifying an alternative path
other than `BAT0` (this may be necessary if running Linux in a VM) or for specifying the path to a secondary battery. A sample usage in tmux is

```
set -g status-right "#([ -d /sys/class/power_supply/BAT1 ] && /path/to/battery -t -b /sys/class/power_supply/BAT1) #(/path/to/battery -t)"
```

This will show the battery indicator only if BAT1 is present.

You can specifiy the colors for __good__ battery level, __middle__ battery level, and __warning__ battery level with the flags ``` -g -m -w ```.
__Note:__ You should use color names for when in tmux mode and [ascii colors](http://www.termsys.demon.co.uk/vtansi.htm#colors) in terminal mode.
