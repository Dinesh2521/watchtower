Watchtower
==========
Chris Lane  
26 Apr 2012
chris@chris-allen-lane.com  
http://chris-allen-lane.com  
http://twitter.com/#!/chrisallenlane


What it Does
------------
`Watchtower` is a [Static Code Analysis](http://en.wikipedia.org/wiki/Static_program_analysis)
tool designed to assist security auditors who are tasked with performing
manual code reviews. It is platform- and language-agnostic.


Usage
-----
There are essentially two use-cases for `Watchtower`:

### Importing Scan Data into You Own Application
`Watchtower` can be configured (via command-line options) to output its
analysis data as CSV, XML, or plain text:

	# to output a CSV:
	./watchtower -s /app/path/to/scan -o csv > /path/to/report/file.csv

	# to output XML:
	./watchtower -s /app/path/to/scan -o xml > /path/to/report/file.xml

	# to output plain text:
	./watchtower -s /app/path/to/scan -o txt

The CSV format is useful for importing scan data into a spreadsheet. The
XML format is useful for importing scan data into a custom application. TXT
is less likely to be useful, but it's there if you need it. (You can also
colorize plain-text data with the `--colorize` flag, if you're outputting
directly to the terminal.)

### Outputting a Sortable HTML Report for Direct Review
In my opinion, `Watchtower`'s greatest feature is its ability to output
an HTML report. Such a report can be a tremendous timesaver when scanning
for a large number of payloads in large numbers of files.

This report is especially useful when running dual monitors, such that
you can have the report open on one monitor, and your preferred IDE
open on the other.

To generate the HTML report, run:
	
	./watchtower -s /app/path/to/scan -o html > /path/to/report/file.html -p 'The Project Name'

    
Configuration
-------------
`Watchtower` has an extensive configuration file. (A large number of
configuration options have to do with formatting and content for HTML
reporting.) Please read the comments in the config file itself
(`.config.rb`) for more information.

Also, note that it's possible to specify which config file to use when
executing `Watchtower` via the `--config-file` option. This is useful
when you're working on several projects at once. Rather than having
to repeatedly change config values, you may simply create alternate
config files (`config-project-one.rb`, etc), and then specify which to
use upon execution.


Payload Specification
---------------------
Payloads (by default) live in <project root>/payloads/, and are specified
by file type, and then by group, thusly:

	$payloads[:filetype][:group]
	
As in:

	$payloads[:php][:dangerous_functions] = %w[payload payload payload ...]

Payload group divisions will be respected when laying out a generated
HTML report, so creating thoughtful groupings can help to make reports
more navigable. Use them wisely.

If you're interested in creating a payload for a new file type, do the following:

	# run this in a terminal:
	ack-grep --help-types

You'll need to use the ack-grep option name (symbolized) as the key
for your new payload. Thus `--java` will translate to `:java`.

Once you've determined the proper key to use for your new filetype, just
create a hash that is structured based off of one of the existing
payloads.


Known Issues
------------
This program has only been tested on Ubuntu 11.04, and may not work on
other platforms. Additionally:

* Watchtower is currently unable to recognize ASP/X files, because `ack-grep`
  (upon which this program relies) itself cannot. I plan to fix
  this in the future.

* If you attempt to generate the same report more than once, you may find
  that the ordering of the elements contained therein will change. (Only
  the ordering of the information will change - the informational content
  itself will not.) This is because my Ruby-Fu is still weak, and I haven't
  yet figured out how to sort some of the nested hashes which the `VulnScanner`
  model assembles before outputting a report. I plan to address this issue
  very soon.
  
* I need to put more work into writing thoughtful default payload lists.
  I will do so in the near-term.


Contact Me
----------
Questions, concerns, bug reports, and feature requests may be directted to
chris@chris-allen-lane.com. If you're able to express your thoughts in
144 characters or less ("It's great!", "It sucks!") you can also contact
me on [twitter](http://twitter.com/#!/chrisallenlane).


License
-------
This product is licensed under the [GPL 3](http://www.gnu.org/copyleft/gpl.html),
and comes with absolutely no warranty, expressed or implied. See the LICENSE file
for more information.
