MAIN = report
CC = pdflatex
FLAGS = 
TEMP = *.aux *.log *.out *.toc *.bbl *.blg *.nav *.snm *.listing
PARTS=parts

all: compress

$(MAIN).pdf: $(MAIN).tex $(PARTS)/*.tex $(PARTS)/*.bib auto/*.tex
	$(CC) $(FLAGS) $<
	-bibtex $(MAIN).aux
	$(CC) $(FLAGS) $<
	$(CC) $(FLAGS) $<
	make hide

compress: $(MAIN).pdf
	gs -sDEVICE=pdfwrite \
    -dCompatibilityLevel=1.4 \
    -dNOPAUSE \
    -dOptimize=true \
    -dQUIET \
    -dBATCH \
    -dRemoveUnusedFonts=true \
    -dRemoveUnusedImages=true \
    -dOptimizeResources=true \
    -dDetectDuplicateImages \
    -dCompressFonts=true \
    -dEmbedAllFonts=true \
    -dSubsetFonts=true \
    -dPreserveAnnots=true \
    -dPreserveMarkedContent=true \
    -dPreserveOverprintSettings=true \
    -dPreserveHalftoneInfo=true \
    -dPreserveOPIComments=true \
    -dPreserveDeviceN=true \
    -dMaxInlineImageSize=0 \
    -sOutputFile="$(MAIN)_compressed.pdf" \
    "$(MAIN).pdf"
	rm $(MAIN).pdf
	mv $(MAIN)_compressed.pdf $(MAIN).pdf

hide:
	mkdir -p out && \
	for ext in $(TEMP); do \
		files=$$(ls $$ext 2>/dev/null) && mv -f $$files out/ 2>/dev/null || true; \
	done
	
	mkdir -p out/$(PARTS) && \
	for ext in $(TEMP); do \
		files=$$(ls $(PARTS)/$$ext 2>/dev/null) && mv -f $$files out/$(PARTS) 2>/dev/null || true; \
	done

clean:
	@rm -f $(TEMP)
	@cd $(PARTS) && rm -f $(TEMP)
	@rm -rf out

.PHONY: all clean hide