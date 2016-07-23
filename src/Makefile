#!/bin/bash
# program settigs
TEX	=platex
MAKEIDX	=mendex -d jisyo.dic 
BIBTEX	=jbibtex
DVIPDF	=dvipdfmx
DVIPS	=dvips -Ppdf 
# filename setting
FILE	=ron
VERSION =3.1
WEBPDF	=$(FILE)hyper
REF	=ron.bib
IST	=sind.ist
GST	=sgls.ist
BST	=splain.bst
SRC	=ipa.map jisyo.dic 
SRC2	=okuduke.tex 002preface.tex 003thanx.tex 01pregame.tex \
	02pre.tex 03text.tex 04biblio.tex 05output.tex 06ml.tex \
	07math.tex 08graphic.tex 10apply.tex 14sample.tex \
	fdl.tex fdl.ja.tex flow.tex okuduke.tex pages.tex $(FILE).bbl
STY	=indent.sty \
	ronmodi.sty \
	ronhyset.tex \
	pict2e.sty 
MAC	=ronmac
# default `all' target is $(FILE).dvi
all: $(FILE).pdf

$(MAC).sty:	$(MAC).dtx $(MAC).ins
	$(TEX) $(MAC).ins
$(MAC).dvi:	$(MAC).dtx
	$(TEX) $(MAC).dtx && $(TEX) $(MAC).dtx
	rm -f $(MAC).aux $(MAC).log 

$(FILE).pdf:	$(FILE).dvi
	$(DVIPDF) $(FILE)
pdf:	$(FILE).dvi
	dvipdfmx -f ipa.map -r 2400 -o $(FILE)-ipa.pdf $(FILE)
$(FILE).ps:	$(FILE).dvi
	$(DVIPS) -o $(FILE).ps $(FILE)
$(FILE).dvi:	$(FILE).ind #$(FILE).bbl #$(FILE).gls
	$(TEX) $(FILE) && $(TEX) $(FILE)
	(while egrep "may have changed" $(FILE).log; \
           do $(TEX) $(FILE); done)
$(FILE).ind:	$(FILE).idx 
	$(MAKEIDX) -s $(IST) -o $(FILE).ind $(FILE).idx
$(FILE).gls:	$(FILE).glo
	$(MAKEIDX) -s $(GST) -o $(FILE).gls $(FILE).glo
$(FILE).bbl:	$(FILE).aux
	$(BIBTEX) $(FILE)
$(FILE).aux $(FILE).idx $(FILE).glo:	$(FILE).tex $(SRC) $(STY)
	$(TEX) $(FILE)

$(WEBPDF).pdf:	$(FILE).tex

	echo "\AtBeginDvi{\special{pdf:tounicode EUC-UCS2}}"\
	> $(WEBPDF).tex
	grep "%" $(FILE).tex >> $(WEBPDF).tex
	$(TEX) $(WEBPDF)
#	$(BIBTEX) $(WEBPDF)
	$(TEX) $(WEBPDF)
	$(MAKEIDX) -s $(IST) -o $(WEBPDF).ind $(WEBPDF).idx 
#	$(MAKEIDX) -s $(GST) -o $(WEBPDF).gls $(WEBPDF).glo
	$(TEX) $(WEBPDF); $(TEX) $(WEBPDF); $(DVIPDF) $(WEBPDF)

tar:
	mkdir -p $(FILE)-$(VERSION)/images
	cp images/* $(FILE)-$(VERSION)/images/
	cp $(REF) $(SRC) $(SRC2) $(IST) $(GST) $(BST) $(STY) \
		$(FILE).tex Makefile $(FILE).bbl $(FILE)-$(VERSION)/
	tar cjf $(FILE)-$(VERSION).tar.bz2 $(FILE)-$(VERSION)/
	rm -fr $(FILE)-$(VERSION)

# clean temporary files instead of $(FILE).log
clean:
	rm -f $(FILE).blg $(FILE).lof $(FILE).lot
	rm -f $(FILE).out $(FILE).toc $(FILE).ind $(FILE).aux
	rm -f $(FILE).dvi $(FILE).idx $(FILE).ilg *.aux
cleanhyper:
	rm -f $(WEBPDF).bbl $(WEBPDF).blg $(WEBPDF).lof $(WEBPDF).lot
	rm -f $(WEBPDF).out $(WEBPDF).toc $(WEBPDF).ind $(WEBPDF).aux
	rm -f $(WEBPDF).dvi $(WEBPDF).idx $(WEBPDF).ilg $(WEBPDF).brf
	rm -f *.aux 
# clean any temporary files
cleanany:	clean cleanhyper
	rm -f *.aux
	rm -f *.log 

