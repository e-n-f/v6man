SHELL = /bin/sh

DOCS = cover.ps start.ps unix.ps c.ps ctut.ps as.ps ed.ps beg.ps \
	rat.ps yacc.ps iosys.ps secur.ps dc.ps bc.ps iolib.ps hel.ps

# the nroff, tmg, m6, and eqn papers are missing

GROFF = groff -wall
#GROFF = groff -Tascii 

all.ps: docs.ps frontmatter.ps man.ps license.ps
	./catps license.ps frontmatter.ps man.ps docs.ps > all.ps

dist: clean
	dir=`basename \`pwd\``; cd ..; tar vcf $$dir.tar $$dir

clean:
	rm -f *.ps man/man?/*.ps

docs.ps: $(DOCS)
	./catps $(DOCS) > docs.ps

license.ps: UNIX_LICENSE
	(echo ".LP"; cat UNIX_LICENSE) | $(GROFF) -ms > license.ps

frontmatter.ps:
	cat man/man0/intro man/man0/basinf | sed 's/_/\\(mi/g' | \
		$(GROFF) > frontmatter.ps

man.ps:
	for i in man/man?/*.[0-9]; \
	do \
		if test ! -f $$i.ps; \
		then \
			echo $$i; \
			./fixroff $$i | sed 's/^\.\(..\)\([^ ]\)/.\1 \2/' | \
			case $$i in \
				*eqn*) eqn ;; \
				*diff*) sed 's/\\\*v/|/' ;; \
				*printf*) sed 's/\\fb/\\fB/' ;; \
				*ecvt*) sed 's/(\*_/_/' ;; \
				*greek*) sed 's/^\.ta.*/.ta 1i 1.5i 2i 3i 3.5i 4i 5i 5.5i 6i/' | tr -d '\016\017' ;; \
				*) cat ;; \
			esac | $(GROFF) man/man0/taa - > $$i.ps; \
		fi; \
	done
	./catps man/man?/*.ps > man.ps

as.ps:
	./fixroff as/* | sed 's/_/\\(em/g' | $(GROFF) > as.ps

bc.ps:
	cat bc/bc | sed 's/\.IP/& /' | $(GROFF) -ms > bc.ps

beg.ps:
	cat beg/u.mod beg/u? | sed 's/_/\\(em/g' | $(GROFF) -ms > beg.ps

c.ps:
	( echo ".ftr G CR"; cat c/* ) | sed 's/^\.ti/& /' | \
		sed '/^[.'"'"']tr ^\\|/,/^[.'"'"']tr ^^/s/\^/\\|/g' | \
		sed '/^[.'"'"']tr \\|\\|/d' | \
		sed 's/\\\* /\** /' | \
		sed 's/_/\\(em/' | $(GROFF) > c.ps

dc.ps:
	( echo ".ds _ _"; cat bc/dc ) | sed 's/\.IP/& /' | $(GROFF) -ms > dc.ps

cover.ps:
	$(GROFF) start/cover > cover.ps

ctut.ps:
	( echo ".ftr G CR"; echo ".tr __"; cat ctut/* ) | sed 's/\.tr/& /' | \
		grep -v '^\.fp' | sed 's/\.tr *__/.tr _\\(em/g' | \
		$(GROFF) > ctut.ps

ed.ps:
	( echo ".ds . \\&."; ./fixroff ed/* ) | sed 's/^\.tr/& /' | \
		$(GROFF) -ms > ed.ps

hel.ps:
	./fixroff hel/hel* | $(GROFF) -C > hel.ps

iolib.ps:
	cat iolib/* | sed 's/^\.ti 5/.in 5/' | $(GROFF) -ms > iolib.ps

iosys.ps:
	$(GROFF) iosys/* > iosys.ps

rat.ps:
	( echo ".nf"; echo '.tr _\(em'; cat rat/* ) | $(GROFF) -ms > rat.ps

secur.ps:
	( echo ".tr _\(em"; cat secur/* ) | $(GROFF) > secur.ps

start.ps:
	./fixroff start/start | $(GROFF) -ms > start.ps

unix.ps:
	$(GROFF) unix/* > unix.ps

yacc.ps:
	$(GROFF) -ms yacc/* > yacc.ps
