# Collectd Python Plugins

This is a collections of Python plugin for Collectd.

- `cpu_temp.py`: Report the CPU temperature. Tested on a Raspberry Pi 3.
- `sht21_kernel.py`/`sht21_usermode.py`: Measure temperature and relative
  humidity from a Sensirion SHT21 sensor connected via I²C. Calculate dew
  point and absolute humidity. Tested on a Raspberry Pi 3.
- `mcp3425.py`: Measure voltage using an MCP3425 analog-digital converter.

For more information, please refer to [my
blogpost](https://blog.dbrgn.ch/2017/3/10/write-a-collectd-python-plugin/).

## Configuration

Copy the desired Python files to your target system. Then add the module to
your `collectd.conf`. Make sure to adjust the `ModulePath` value. The following
example assumes the plugins were copied to `/opt/collectd_plugins`.

### cpu_temp

If your CPU temperature cannot be read from
`/sys/class/thermal/thermal_zone0/temp`, make sure to adjust that variable too.

    LoadPlugin python
    <Plugin python>
        ModulePath "/opt/collectd_plugins"
        Import "cpu_temp"
        <Module cpu_temp>
            Path "/sys/class/thermal/thermal_zone0/temp"
        </Module>
    </Plugin>

### sht21_kernel

For this plugin to work, the `sht21` kernel module must be loaded:

    echo "sht21" > /etc/modules-load.d/sht21.conf

There are currently no configuration options available.

    LoadPlugin python
    <Plugin python>
        ModulePath "/opt/collectd_plugins"
        Import "sht21_kernel"
    </Plugin>

### sht21_usermode

Same as `sht21_kernel`, but it does not require the `sht21` kernel module.  
The downside of the user-mode plugin is that it does not allow concurrent
access. As workaround, you can specify a lock file shared by
concurrent applications.  

    LoadPlugin python
    <Plugin python>
        ModulePath "/opt/collectd_plugins"
        Import "sht21_sht21_usermode"
        <Module sht21_usermode>
            LockFile "/var/run/lock/sht21.lock"
        </Module>
    </Plugin>

### mcp3425

The plugin assumes that you're using three voltage divider resistors to bring
the voltage into a measurable range. You can configure them in the Python
script.

There are currently no configuration options available.

    LoadPlugin python
    <Plugin python>
        ModulePath "/opt/collectd_plugins"
        Import "mcp3425"
    </Plugin>

## License

MIT License, see LICENSE file.
