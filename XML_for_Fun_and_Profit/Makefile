################################################################################
#
# File: $Source$
#
# $Id$
#
################################################################################

SHELL = /bin/sh

.SUFFIXES:
.SUFFIXES: .pdf .tex .png .tiff .gif .sxd


# directories
IMGDIR = images


# paths
vpath %.pdf $(IMGDIR)
vpath %.png $(IMGDIR)
vpath %.tif $(IMGDIR)
vpath %.gif $(IMGDIR)
vpath %.sxd $(IMGDIR)


# files
SLIDE_IMAGES = NZCSbanner.png w3-shot.png

SLIDE_BACKGROUNDS = gopher-BG.png

FIG_IMAGES = $(BTREE) $(INDEXORGANISED) $(REVERSEKEY) \
	OracleArchitecture.pdf OracleStructures.pdf OrderERD.pdf
	


# miscellaneous variables
# say make DRAFT=draft xxx if you want to run in draft mode
DRAFT =
# LaTeX options (comma-separated list)
LATEX_OPTS = pdftex,$(DRAFT)

SLIDES = XML_for_Fun_and_Profit
NOTES = XML_for_Fun_and_Profit_notes

TARGETS = all slides notes slides-notes test clean tidy targets


.PHONY: $(TARGETS)


# everything
all: slides notes


# slides for presentation
slides: $(SLIDES).pdf

$(SLIDES).pdf: $(SLIDES).tex $(SLIDE_IMAGES) $(SLIDE_BACKGROUNDS) beamerthemenzcs.sty
	pdflatex --jobname=$(SLIDES) '\documentclass[$(LATEX_OPTS)]{beamer}\input{$(SLIDES)}'
	pdflatex --jobname=$(SLIDES) '\documentclass[$(LATEX_OPTS)]{beamer}\input{$(SLIDES)}'


# slides with notes
slides-notes: slides-notes.pdf

slides-notes.pdf: $(SLIDES).tex $(SLIDE_IMAGES) beamerthemenzcs.sty
	pdflatex --jobname=slides-notes '\documentclass[$(LATEX_OPTS),notes=onlyslideswithnotes]{beamer}\input{$(SLIDES)}'
	pdflatex --jobname=slides-notes '\documentclass[$(LATEX_OPTS),notes=onlyslideswithnotes]{beamer}\input{$(SLIDES)}'


# slides with notes, 6-up
notes: $(NOTES).pdf

$(NOTES).pdf: slides-notes.pdf
	pdfnup --nup 2x3 --outfile $@ $<


# images
gopher.png: gopher.tif
	convert -blur 0.5 $< $(IMGDIR)/$@

%-BG.png: %.tif
	convert -blur 0.5 $< $(IMGDIR)/$*.png
	convert $(IMGDIR)/$*.png -threshold 0,0 -depth 16 - | composite -dissolve 15% $(IMGDIR)/$*.png - $(IMGDIR)/$@

# %-BG.jpg: $(ANALOGY_IMGDIR)/%.jpg
# 	convert $< -threshold 0,0 -depth 16 - | composite -dissolve 15% $< - $(IMGDIR)/$@

NZCSbanner.png: NZCSbanner-original.gif
	convert -crop 737x80+0+0 $< $(IMGDIR)/$@

# $(BTREE): BTreeParameters.graffle
# 	@echo "$(notdir $<): regenerate $(notdir $@)"
# 	@exit 1
# 
# $(REVERSEKEY): ReverseKeyIndexes.graffle
# 	@echo "$(notdir $<): regenerate $(notdir $@)"
# 	@exit 1
# 
# $(INDEXORGANISED): IndexOrganisedTables.graffle
# 	@echo "$(notdir $<): regenerate $(notdir $@)"
# 	@exit 1
# 
# $(PHYSICALSTORAGE): PhysicalStorage.graffle
# 	@echo "$(notdir $<): regenerate $(notdir $@)"
# 	@exit 1
# 
# $(CLUSTERS): Clustering.graffle
# 	@echo "$(notdir $<): regenerate $(notdir $@)"
# 	@exit 1


# test document
test: test.pdf

test.pdf: test.tex


# clean up: get rid of all the temporary files
tidy:
	rm -f *.dvi *.tmp *.aux *.out *.log *.nav *.toc *.snm \
		*.head slides-driver.tex slides-handout.pdf \
		$(IMGDIR)/*-BG.*

# clean up: get rid of everything except the original source
clean: tidy
	rm -f *.pdf


# list all targets
targets:
	@echo "targets: $(TARGETS)"


# default rules
%.png: %.tif
	convert "$<" $(IMGDIR)/$@

%.pdf: %.sxd
	@announce "Remember to close OpenOffice first!!"
	soffice -headless -display :0 -pt PS_EPS_PDF_Converter $<
	sleep 5; mv ~/$(@:.pdf=.sxd.pdf) $(IMGDIR)/$@
