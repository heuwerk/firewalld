XSLTPROC = xsltproc

if ENABLE_DOCS
EXTRA_DIST = $(HTMLS:../html/%.html=%.xml) \
	authors.xml notes.xml seealso.xml errorcodes.xml \
	transform-man.xsl.in transform-html.xsl.in \
	firewalld.xml.in firewall-cmd.xml.in \
	policy_zone_syntax.xml policy_zone_descriptions.xml

man_MANS = $(man1_MANS) $(man5_MANS)
HTMLS = $(man1_MANS:../man/man1/%.1=../html/%.html) $(man5_MANS:../man/man5/%.5=../html/%.html)

man1_MANS = \
	../man/man1/firewall-applet.1 \
	../man/man1/firewall-cmd.1 \
	../man/man1/firewall-config.1 \
	../man/man1/firewalld.1 \
	../man/man1/firewall-offline-cmd.1

man5_MANS = \
	../man/man5/firewalld.conf.5 \
	../man/man5/firewalld.dbus.5 \
	../man/man5/firewalld.direct.5 \
	../man/man5/firewalld.helper.5 \
	../man/man5/firewalld.icmptype.5 \
	../man/man5/firewalld.ipset.5 \
	../man/man5/firewalld.richlanguage.5 \
	../man/man5/firewalld.service.5 \
	../man/man5/firewalld.zone.5 \
	../man/man5/firewalld.zones.5 \
	../man/man5/firewalld.policy.5 \
	../man/man5/firewalld.policies.5
endif

CLEAN_FILES = *~ errorcodes.xml
DISTCLEANFILES = $(man_MANS) $(HTMLS) transform-*.xsl \
                 firewalld.xml firewall-cmd.xml

#SGML_CATALOG_FILES
#XSLTPROC_FLAGS = --catalogs --nonet --xinclude
XSLTPROC_FLAGS = --nonet --xinclude
XSLTPROC_MAN_FLAGS = $(XSLTPROC_FLAGS) transform-man.xsl
XSLTPROC_HTML_FLAGS = $(XSLTPROC_FLAGS) transform-html.xsl

install:

all: $(man_MANS) $(HTMLS)

clean:
	-test -z "$(CLEAN_FILES)" || rm -f $(CLEAN_FILES)

../man/man1/firewall-cmd.1: errorcodes.xml
../html/firewall-cmd.html: errorcodes.xml

../man/man1/%.1: %.xml authors.xml notes.xml seealso.xml transform-man.xsl policy_zone_syntax.xml policy_zone_descriptions.xml
	$(XSLTPROC) -o $@ $(XSLTPROC_MAN_FLAGS) $<

../man/man5/%.5: %.xml authors.xml notes.xml seealso.xml transform-man.xsl policy_zone_syntax.xml policy_zone_descriptions.xml
	$(XSLTPROC) -o $@ $(XSLTPROC_MAN_FLAGS) $<

../html/%.html: %.xml authors.xml notes.xml seealso.xml transform-html.xsl policy_zone_syntax.xml policy_zone_descriptions.xml
	mkdir -p $(dir $@) # avoid xsltproc directory create race
	$(XSLTPROC) -o $@ $(XSLTPROC_HTML_FLAGS) $<

errorcodes.xml: ../../src/firewall/errors.py
	@echo Creating $@
	@grep '=\s*[0-9]\+$$' ../../src/firewall/errors.py | \
		sed -e 's/^/\<row\>\<entry\>/g' \
			-e 's/ *= */\<\/entry\>\<entry\>/g' \
			-e 's/$$/\<\/entry\>\<\/row\>/g' > $@

edit = sed \
	-e 's|\@PREFIX\@|$(prefix)|' \
	-e 's|\@SYSCONFDIR\@|$(sysconfdir)|' \
	-e 's|\@PACKAGE_STRING\@|$(PACKAGE_STRING)|' \
	-e 's|\@IFCFGDIR\@|$(IFCFGDIR)|' \
	-e 's|@SRCDIR@|$(srcdir)|'

transform-man.xsl: transform-man.xsl.in
	$(edit) $< >$@
transform-html.xsl: transform-html.xsl.in
	$(edit) $< >$@
firewall-cmd.xml: firewall-cmd.xml.in
	$(edit) $< >$@
firewalld.xml: firewalld.xml.in
	$(edit) $< >$@
