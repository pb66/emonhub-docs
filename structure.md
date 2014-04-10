emonHub structure
============

The emonHub system is specifically not designed to be run directly from a cloned git repository. 

This is a deliberate choice to ensure that the file structure and version control specifics are not tightly coupled to running installations, which makes refactoring without breaking these installations impractical.

##What does what and where does it go?##

Version 1 proposed filesystem:

 - `conf/`
   - `conf/emonhub.conf` : configuration for emonhub itself
   - `conf/default/emonhub` : configuration for the init.d service script
 - `service/`
   - `service/emonhub` : init.d service script (controls the emonhub daemon)
 - `src/`
   - `emonhub.py` : the core emonhub source module. 
   - _other Python source files as required_

##Deployed filesystem##

Once installed, the following will be observed:

- `service/emonhub` is deployed as `/etc/init.d/emonhub`
- `conf/default/emonhub` is deployed as `/etc/default/emonhub`
- `src/*.py` is deployed as `/usr/share/emonhub/*.py`
- `conf/emonhub.conf` is deployed as `/etc/emonhub/emonhub.conf`

Using default config, emonhub will begin logging to `/var/log/emonhub/emonhub.log`

## Development tips ##

The simplest way to work with this structure on a development machine is to symlink the files from the repository onto the final destinations, for example by running `sudo ln -s /full/path/to/src /usr/share/emonhub` (which would mean your `src/` directory becomes "live", and once satisfied you can just do a `git commit` on the repository).

An alternative is to edit the various configuration parameters to point the daemon at your repository (i.e. by editing `EMONHUB_PATH` in `/etc/default/emonhub` to point to `/your/full/repo/path/src`)

Don't forget to reinstate to ensure all your changes work together, prior to pushing changes up for others to use.
