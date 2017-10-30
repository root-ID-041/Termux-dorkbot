dorkbot
=======

Scan Google search results for vulnerabilities.

dorkbot is a modular command-line tool for performing vulnerability scans against a set of webpages returned by Google search queries in a given Google Custom Search Engine. It is broken up into two sets of modules:

* *Indexers* - modules that issue a search query and return the results as targets
* *Scanners* - modules that perform a vulnerability scan against each target

Targets are stored in a local database upon being indexed. Once scanned, any vulnerabilities found by the chosen scanner are written to a standard JSON report file. Indexing and scanning processes can be run separately or combined in a single command.

Usage
=====
<pre>
usage: dorkbot.py [-h] [-c CONFIG] [-b BLACKLIST] [-d DATABASE] [-i INDEXER]
                  [-l] [-o INDEXER_OPTIONS] [-p SCANNER_OPTIONS] [-s SCANNER]
                  [-v VULNDIR]

optional arguments:
  -h, --help            show this help message and exit
  -c CONFIG, --config CONFIG
                        Configuration file
  -b BLACKLIST, --blacklist BLACKLIST
                        File containing (regex) patterns to blacklist from
                        scans
  -d DATABASE, --database DATABASE
                        SQLite3 database file
  -i INDEXER, --indexer INDEXER
                        Indexer module to use
  -l, --list            List targets in database
  -o INDEXER_OPTIONS, --indexer-options INDEXER_OPTIONS
                        Indexer-specific options (opt1=val1,opt2=val2,..)
  -p SCANNER_OPTIONS, --scanner-options SCANNER_OPTIONS
                        Scanner-specific options (opt1=val1,opt2=val2,..)
  -s SCANNER, --scanner SCANNER
                        Scanner module to use
  -v VULNDIR, --vulndir VULNDIR
                        Directory to store vulnerability output reports
</pre>

Platform
========
Python 2.7.x / 3.x (Linux / Mac OS / Windows)
(requires [python-dateutil](https://pypi.python.org/pypi/python-dateutil))

Quickstart
==========
1. Download [PhantomJS](http://phantomjs.org/) and either [Arachni](http://www.arachni-scanner.com/) or [Wapiti](http://wapiti.sourceforge.net/) for your platform, and make sure you have installed any required dependencies for each.
1. Extract each tool into the tools directory and rename the directory after the tool (dorkbot/tools/phantomjs/, dorkbot/tools/arachni/, etc).
1. Create a Google [Custom Search Engine](https://www.google.com/cse/) and note the search engine ID, e.g. 012345678901234567891:abc12defg3h.
1. Install python-dateutil (e.g.: pip install python-dateutil)


Example: use arachni to scan php pages that contain the string "id" in the url:
<pre>
$ ./dorkbot.py -i google -o engine=012345678901234567891:abc12defg3h,query="filetype:php inurl:id" -s arachni
</pre>

Indexer Modules
===============
### google ###
Search for targets in a Google Custom Search Engine (CSE) via custom search element.

Requirements: [PhantomJS](http://phantomjs.org/)

Options:
* **engine** - CSE id
* **query** - search query
* phantomjs_dir - phantomjs base directory containing bin/phantomjs (default: tools/phantomjs/)
* domain - limit searches to specified domain

### google_api ###
Search for targets in a Google Custom Search Engine (CSE) via JSON API.

Requirements: none

Options:
* **key** - API key
* **engine** - CSE id
* **query** - search query
* domain - limit searches to specified domain

### stdin ###
Read targets from standard input, one per line.

Requirements: none

Options: none

Scanner Modules
===============
### arachni ###
Scan targets with Arachni command-line scanner.

Requirements: [Arachni](http://www.arachni-scanner.com/)

Options:
* arachni_dir - arachni base directory containing bin/arachni and bin/arachni_reporter (default: tools/arachni/)
* report_dir - directory to save arachni scan binary and JSON scan report output (default: reports/)
* checks - which vulnerability checks to perform (default: active/\*,-csrf,-unvalidated_redirect,-source_code_disclosure,-response_splitting,-no_sql_injection_differential

### wapiti ###
Scan targets with Wapiti command-line scanner.

Requirements: [Wapiti](http://wapiti.sourceforge.net/)

Options:
* wapiti_dir - wapiti base directory containing bin/wapiti (default: tools/wapiti/)
* report_dir - directory to save wapiti JSON scan report (default: reports/)

# Termux-dorkbot
