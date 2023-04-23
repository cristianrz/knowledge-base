# OpenSCAP

## Install packages

```bash
yum install openscap-scanner scap-security-guide
```

## Check for compliance

### Level 2

```bash
oscap xccdf eval \
	--profile xccdf_org.ssgproject.content_profile_cis \
	--results scan_results.xml \
	--report scan_report.html \
	/usr/share/xml/scap/ssg/content/ssg-almalinux8-ds.xml
```

### Level 1

```bash
oscap xccdf eval \
	--profile xccdf_org.ssgproject.content_profile_cis_server_l1 \
	--results scan_results.xml \
	--report scan_report.html \
	/usr/share/xml/scap/ssg/content/ssg-almalinux8-ds.xml
```

## Fix

```
oscap xccdf generate fix \
	--fix-type bash \
	--output fix.sh \
	--result-id "" \
	scan_results.xml
```