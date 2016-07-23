.SUFFIXES: .tex .aux .bib .blg .bbl .toc .log .dvi .ps .pdf
F=	sample
main=	$F.tex
persons=001_Taro_Hakodate.tex 002_Hanako_Mirai.tex 003_Michio_Hokkai.tex
all:	$F.pdf
$F.pdf: $F.dvi
	platex $*
	dvipdfmx -o $@ $*
$F.ps:  $F.dvi
	platex $*
	dvips -Ppdf -o $@ $<
$F.dvi: $(main)
	platex $*
	for person in $(persons); do \
		jbibtex `basename $$person .tex`; \
	done
	platex $*
clean:
	rm -f $F.dvi *.log *.blg *.aux