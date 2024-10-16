# mod_log_rotate

Original from andya ät apache.org

Modified version from Mario Brandt / JBlond

## INTRODUCTION

If you host a lot of virtual servers on a single Apache box and use the supplied rotatelogs
program to rotate the access logs you'll notice that your process table is cluttered up with an instance
of rotatelogs for each virtual server. With mod_log_rotate the log rotation is handled by the server
process so you save a bunch of processes and file descriptors.

Note: error log files can not be rotated with this module.

## BUILD

See [build.md](build.md)

## INSTALL

	Copy mod_log_rotate.so to your Apache 2.4.x modules folder
	.../apache24/modules/mod_log_rotate.so

## USAGE:

```
LoadModule log_rotate_module modules/mod_log_rotate.so
RotateLogs On
#RotateInterval 86400
#RotateInterval 100 MB

```

You can specify a strftime() format string as the log file name.

```
CustomLog logs/%Y%m%d-%H%M%S.access.log common
```

## CONFIGURATION DIRECTIVES:

	RotateLogs On|Off   Enable / disable automatic log rotation. Once enabled
						mod_log_rotate takes responsibility for all log output
						server wide even if RotateLogs Off is subsequently
						used. That means that the BufferedLogs directive that
						is implemented by mod_log_config will be ignored.

	RotateLogsLocalTime Normally the log rotation interval is based on UTC.
						For example an interval of 86400 (one day) will cause
						the logs to rotate at UTC 00:00. When this option is
						on, log rotation is timed relative to the local time.

	RotateInterval      Set the interval in seconds for log rotation. The
						default is 86400 (one day). The shortest interval that
						can be specified is 60 seconds. An optional second
						argument specifies an offset in minutes which is
						applied to UTC (or local time if RotateLogsLocalTime
						is on). For example RotateInterval 86400 60 will
						cause logs to be rotated at 23:00 UTC.
						If the interval is set to 0, the log will not be written at
						all, similar like routating to /dev/null.

						If the interval option is set to KB/MB/GB, the log will base
 						on size, the log name will base on 10 seconds. For example
						RotateInterval 100 MB will casue logs to be rotated when
						100MB data is writen. The logfile size will be equal or
						less than 100 MB.

## Bugs

A wrong configured placeholder for strftime causes a crash in the module.
So if you use Y% instead of %Y apache crashes with a segfault. This bug is confirmed on Windows
and might happen on *nix based systems, too.
On Windows it is the call of strftime in timestr.c (APR) line ~ 190 [win32_strftime_extra]


## AVAILABILITY

### AVAILABILITY of the original

mod_log_rotate.c original source code under the Apache License Version 2.0
is no longer available.

### AVAILABILITY of the modified version

https://github.com/sunnyqeen/mod_log_rotate

https://github.com/JBlond/mod_log_rotate

### AVAILABILITY Windows binary x86 and x64

https://www.apachehaus.com/cgi-bin/download.plx

## Contribute

Contributers are welcome. Open Pull request or file an issue.
