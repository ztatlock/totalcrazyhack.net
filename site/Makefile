PAGES=$(wildcard *.md)
STAGE_PAGES=$(patsubst %.md,stage/%.html,$(PAGES))

INCLUDE=$(wildcard include/*)
STAGE_INCLUDE=$(patsubst include/%,stage/%,$(INCLUDE))

THUMB_DIR=$(HOME)/Dropbox/totalcrazyhack.net/thumbs/thumb
FILES_DIR=$(HOME)/Dropbox/totalcrazyhack.net/files

.PHONY: stage
stage: $(STAGE_PAGES) $(STAGE_INCLUDE) stage/thumb stage/files

stage/%.html: %.md TEMPLATE.html REFLINKS
	@echo PANDOC $<
	@pandoc \
		--from=markdown \
		--to=html5 \
		--template=TEMPLATE.html \
		--standalone \
		--fail-if-warnings \
		$< REFLINKS --output=$@

stage/%: include/%
	cp -r $< stage/

stage/thumb:
	[ -d "$(THUMB_DIR)" ] && ln -s "$(THUMB_DIR)" stage/thumb

stage/files:
	[ -d "$(FILES_DIR)" ] && ln -s "$(FILES_DIR)" stage/files

.PHONY: sync
sync:
	rsync \
		--verbose \
		--recursive \
		--copy-dirlinks \
		--delete \
		--force \
		stage/ ztatlock@totalcrazyhack.net:/var/www/totalcrazyhack.net

.PHONY: publish
publish: stage sync

.PHONY: clean
clean:
	rm -rf stage/*
