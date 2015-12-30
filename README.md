# logs - write and roll logs

based in part on savelog in debianutils

A shell script for writing and rolling logs.

## Installation

Plop the script in your bin directory or on your PATH somewhere. It requires
bash.

## Usage

This script writes out standard input to a specified log file, but also
handles rolling over old copies of the log file each time it is run. Use it
instead of redirecting to a log file; instead of:

```
<command> > output.log
```

do this:

```
<command> | logs output.log
```

This script runs as long as standard input can be read, so when the feeding
command exits, it does too.

When this script starts, it renames old logs, keeping up to a maximum number
(default 10) around, with the oldest having the highest number in its name. The
sequence is:

```
output.log -> output.log.0.gz -> output.log.1.gz -> ... -> output.log.n-1.gz
```

By default, old logs are compressed using gzip, but different compression or
none at all may be employed.

Run the script with -h to see all the available options.

## Limitations

- If a new log directory is chosen between runs, log rotation starts over.
  Logs in the old log directory are left alone.
- If the maximum count is reduced between runs, old logs with numbering
  greater than one less than the new maximum count are not removed.
- The compression tool must be able to take in the file to compress as its
  sole argument, and compress it in place. gzip, bzip2, xz, and compress
  all qualify.
- The user, group, and mode of old log files are never changed.
