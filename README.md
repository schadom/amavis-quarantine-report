# amavis-quarantine-report

This script generates automated email reports for items in /var/lib/amavis/virusmails/ and allows users to retrieve items from the quarantine.
Has been tested with amavisd-new-2.10.x and postfix 3.x but is currently *not* production ready.

## Install

1) Clone repo via git

2) Create a new postfix alias maps file like the following

	echo "spammgr@yourdomain.com spammgr" >/etc/postfix/amavis_qmgr_transport
	postmap /etc/postfix/amavis_qmgr_transport

3) Add the transport file to `virtual_alias_maps` in main.cf, eg.

	cat /etc/postfix/main.cf
	..
	virtual_alias_maps = hash:${config_directory}/amavis_qmgr_transport
	..
 
4) Add a new entry in /etc/aliases, eg.

        cat /etc/aliases
        ..
	# amavis-quarantine-report
	spammgr: "|/usr/bin/python3 /opt/amavis-quarantine-report/amavis-quarantine-report.py --release"
        ..

5) Run the `newaliases` command


6) Add a cronjob for the automated report generation via `crontab -e`

	5 0 * * * /usr/bin/python3 /opt/amavis-quarantine-report/amavis-quarantine-report.py --send-reports >/opt/amavis-quarantine-report/report.log 

## Todo

- Fix encoding and locale issues
- Better/safer way of releasing quarantined items
- Multi-language email templates
- Clean-up
