SHELL        := /usr/bin/env bash
NAME         := bonanza
VERSION      ?=
BUILD_NUMBER ?= 0
DEB          := dist/$(NAME)_$(VERSION)+$(BUILD_NUMBER)_amd64.deb

guard-%:
	@ if [ "${${*}}" = "" ]; then \
	      echo "Environment variable $* not set"; \
	    exit 1; \
	fi

default: all

all: clean install

init:
	mkdir -p dist/build

.PHONY: clean
clean:
	stack clean
	-rm -rf dist

.PHONY: install
install: init
	stack install --pedantic --test --local-bin-path=dist/build

.PHONY: compile
compile:
	stack build --fast --pedantic --test --no-copy-bins

.PHONY: dist
dist: guard-VERSION $(DEB)

$(DEB): install
	makedeb --name=$(NAME) \
          --version=$(VERSION) \
          --debian-dir=deb \
          --build=$(BUILD_NUMBER) \
          --architecture=amd64 \
          --output-dir=dist
