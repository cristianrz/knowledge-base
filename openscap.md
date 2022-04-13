# OpenSCAP

## Check for compliance

```bash
oscap xccdf eval \
	--profile xccdf_org.ssgproject.content_profile_cis \
	--results scan_results.xml \
	--report scan_report.html \
	/usr/share/xml/scap/ssg/content/ssg-rhel8-ds.xml
```
