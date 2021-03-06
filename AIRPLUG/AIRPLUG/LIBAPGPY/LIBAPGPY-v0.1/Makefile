# license type: free of charge license for academic and research purpose
# see license.txt
# author: Bertrand Ducourthial
# revision: 12/05/2015

# Layer 3 makefile of the Airplug Software Distribution.
# See the file Makefile-var for specific application-dependent variables.
# See the file Makefile-bin for specific application-dependent compiling rules.


### GENERAL PARAMETERS #########################################################
# To avoid any problem from the inheritage of SHELL variable from the
# environment
SHELL = /bin/bash

# Language (if the LANGUAGE env variable contains fr, then french, else english)
TEXT = $(shell if [ x`env | grep LANGUAGE | grep fr` = x ] ; then echo "english" ; else echo "french" ; fi )

# Name of the application (= name of the directory in lower cases)
# NB: It is assumed that the Airplug convention related to directories
#     organisation is satisfied.
# NB: Taking the name of the parent directory to avoid including version number.
OUTPUT = $(shell pwd | rev | cut -d'/' -f 2 | rev | tr '[:upper:]' '[:lower:]')

# Target architecture (for application requiring compilation)
# NB: the output program, the libraries and the object files directories are
#     named according to the ARCH variable used as suffixe. The tgz-bin archive
#     is built according to the ARCH variable (see layer 2 Makefile).
# NB: only if not already defined during a cross-compilation process.
ARCH ?= $(shell uname -m)

# For underline in list rule
BU=`tput smul`
EU=`tput rmul`


### DIRECTORIES NAMES ##########################################################
# NB: It is assumed that the Airplug convention related to directories
#     organisation is satisfied.

# Current directory (without version suffix)
# NB: inherited from parent Makefile (see -e make option in ../Makefile)
# NB: exact directory name to the contrary of OUTPUT (may contain lower cases)
DIR = $(shell pwd | rev | cut -d'/' -f 2 | rev)

# Real current directory name (for display purpose)
DIRNAME = $(shell pwd | rev | cut -d'/' -f 1 | rev)

# Install directory of the whole distribution
export DIR_INSTALL = ../..

# Directory of the C Airplug libraries
DIR_LIBAPG=$(DIR_INSTALL)/LIBAPGC/LIBAPGC


### COMMANDS TO USE ############################################################
# NB: these default values could be overwritten in Makefile-var, which is
#     application dependent

# Make program used for calling sub-makefiles
MAKE = make

# Make options:
#   -s : silent
#   -e : variables which are exported here predominates those initialized in
#        sub-makefiles
MFLAGS = -s -e

# Compiler used for building a compiled application
# NB: ?= means that the variable is defined only if it was not already (in upper
#        layer Makefiles)
# NB: -g is for debog directives. Modifying here CC allows to report the -g
#     option in any sub-makefile without changing CFLAGS that may differ.
CC ?= gcc
#CC = gcc -g

# Name of the file containing the dependencies for appl. Architecture dependent.
MAKEDEPFILE = ./Dependencies-${ARCH}
# Command to generate the dependencies file.
MAKEDEP=gccmakedep -f $(MAKEDEPFILE) -s "\#\#\# Generated by the Airplug Makefile (using make depend) \#\#\#"

# Options added to the C compiler
# NB: This variable is not overridden by parent Makefile thanks to unexport.
# TODO: check the interest and place of unexport in the hierarchie of Makefile...
# TODO: why not using'?=' ? or "CFLAGS = $CFLAGS -I..."
# TODO: add '-fPIC' ? cf. LIBAPGC Makefiles
CFLAGS = -I$(DIR_LIBAPG)/include -I/usr/include/lsb3

# Options added to the linker
# NB: This variable is not overridden by parent Makefile thanks to unexport.
# TODO: check the interest and place of unexport in the hierarchie of Makefile...
# TODO: why not using'?=' ? or "CFLAGS = $CFLAGS -I..."
LDFLAGS =  -L$(DIR_LIBAPG)/lib-$(ARCH) -lapgvrb -lapglst -lapgsgn -lapgiac

# tgif program for generating icons
TGIF = tgif
# convert program for resizing icons
CONVERT = convert

# Files to be ignored
# (used with Makefile command filter-out)
FILTER_OUT= %~ %old %bak %obj


###  APPLICATION DEPENDENT VARIABLES ############################################
# NB: any specific variable is defined in Makefile-var to keep a generic
#     Makefile.
# NB: any previously defined variable could be overwritten in Makefile-var
#     preserving the genericity of the Makefile.
include Makefile-var


### SPECIFIC RULES #############################################################

# To avoid any problem in case a file has the name of a rule
.PHONY: build clean depend \
	files-tgz-bin files-tgz-dev files-tgz-skl files-tgz-pub \
	help icon incr install list mod-version new-version print-tgz-dev \
	reset secret show-version tgz-bin tgz-pub tgz-dev tgz-skl

# First rule = default rule (help)
help:
	@if [ $(TEXT) = "french" ]; then \
		echo "  ++ $(DIR) : aide pour le Makefile de la Suite logicielle Airplug" ; \
		echo "     make build        : compilation" ; \
		echo "     make clean        : suppression des fichiers temporaires" ; \
		echo "     make depend       : calcul des dependances (avant compilation)" ; \
		echo "     make help         : affiche cette aide"; \
		echo "     make icon         : creation de l'icone de l'application" ; \
		echo "     make incr         : similaire a make new-version" ; \
		echo "     make install      : installation du programme (apres compilation)" ; \
		echo "     make list         : liste des fichiers" ; \
		echo "     make mod-version  : modifie la version courante de l'application" ; \
		echo "     make new-version  : cree une nouvelle version de l'application" ; \
		echo "     make reset        : clean + suppression des executables compiles" ; \
		echo "     make secret       : clean + suppression des sources" ; \
		echo "     make show-version : affichage de la version, du type de license et des auteurs" ; \
		echo "     make tgz-bin      : archivage des sources pour distribution binaire" ; \
		echo "     make tgz-pub      : archivage des sources pour distribution publique" ; \
		echo "     make tgz-dev      : archivage des sources pour distribution de developpement" ; \
		echo "     make tgz-skl      : archivage des sources pour embryon de distribution" ; \
	else \
		echo "  ++ $(DIR) : help for Makefile of the Airplug Software Distribution" ; \
		echo "     make build        : compilation" ; \
		echo "     make clean        : delete temporary files" ; \
		echo "     make depend       : compute the dependencies (before compilation)" ; \
		echo "     make help         : display this help"; \
		echo "     make icon         : create the application icon" ; \
		echo "     make incr         : same as make new-version" ; \
		echo "     make install      : install the program (after compilation)" ; \
		echo "     make list         : list the files" ; \
		echo "     make mod-version  : modify the current version of the application" ; \
		echo "     make new-version  : create a new version of the application" ; \
		echo "     make reset        : clean + delete the compiled executables" ; \
		echo "     make secret       : clean + delete sources files" ; \
		echo "     make show-version : display the version, the license type and the authors" ; \
		echo "     make tgz-bin      : archive sources for the binary distribution" ; \
		echo "     make tgz-pub      : archive sources for the public distribution" ; \
		echo "     make tgz-dev      : archive sources for the devel distribution" ; \
		echo "     make tgz-skl      : archive sources for the skeleton distribution" ; \
	fi ;

# NB: version management and packaging rules have no effect here (see layer 2
#     Makefile).
incr mod-version new-version show-version secret tgz-bin tgz-pub tgz-dev tgz-skl:
	@if [ $(TEXT) = "french" ]; then \
		echo "  ++ $(DIRNAME) : regle $@" ; \
		echo "*    sans effet ici" ; \
	else \
		echo "  ++ $(DIRNAME) : $@ rule" ; \
		echo "*    no effect here" ; \
	fi ;

### CLEANING RULES #############################################################
# Rule for deleting all useless files
# NB: objects files of any architectures will be deleted
clean:
	@if [ $(TEXT) = "french" ]; then \
		echo "  ++ $(DIRNAME) : regle $@" ; \
	  echo "     suppression de *~ *.bak *.tgz files-tgz-*.txt obj-*/*.o src/*~ src/*.bak $(MAKEDEPFILE) (s'ils existent)" ; \
	else \
		echo "  ++ $(DIRNAME) : $@ rule" ; \
	  echo "     deleting *~ *.bak *.tgz files-tgz-*.txt obj-*/*.o src/*~ src/*.bak $(MAKEDEPFILE) (if any)" ; \
	fi ; \
	rm -f *~ *.bak *.tgz files-tgz-*.txt obj-*/*.o src/*~ src/*.bak $(MAKEDEPFILE);

# Rule for deleting all useless files as well as the output.
# NB: this rule allows to retrieve the initial state of the package.
reset: 	clean
	@if [ $(TEXT) = "french" ]; then \
		echo "  ++ $(DIRNAME) : regle $@" ; \
		echo "     suppression de $(DIR)/bin/$(OUTPUT)* et de $(DIR)/obj-* (s'ils existent)" ; \
	else \
		echo "  ++ $(DIRNAME) : $@ rule" ; \
		echo "     deleting bin/$(OUTPUT)* and obj-* (if any)" ; \
	fi ; \
	rm -f bin/$(OUTPUT)* ; \
	rm -fr obj-* ;


### PACKAGING RULES #############################################################

# Building the list of files to include in the binary package when called by
# layer 2 Makefile, or binary distribution when called by layer 1 Makefile.
# Depend on the local configuration in Makefile-var.
# NB: not searching for subdirectory src* to the contrary of other packages.
files-tgz-bin:
ifeq ($(strip $(TGZ_BIN)),)
	@if [ $(TEXT) = "french" ]; then \
		echo "  ++ $(DIRNAME) : regle $@" ; \
		echo "*    sans effet ici" ; \
	else \
		echo "  ++ $(DIRNAME) : $@ rule" ; \
		echo "*    no effect here" ; \
	fi ;
else
	@if [ $(TEXT) = "french" ]; then \
		echo "  ++ $(DIRNAME) : regle $@" ; \
	else \
		echo "  ++ $(DIRNAME) : $@ rule" ; \
	fi ; \
	if [ -e ./files-tgz-bin.txt ] ; then \
		rm ./files-tgz-bin.txt ; \
	fi ; \
	for F in $(filter-out $(FILTER_OUT),$(shell ls $(TGZ_BIN) 2> /dev/null)) ; do \
		if [ -e $$F ]; then \
			echo $$F >> ./files-tgz-bin.txt ; \
		else \
			if [ $(TEXT) = "french" ]; then \
				echo "*    $(DIRNAME) : $$F manquant" ; \
			else \
				echo "*    $(DIRNAME) : $$F not found" ; \
			fi ; \
		fi ; \
	done ; \
	if [ $(TEXT) = "french" ]; then \
		echo "     $(DIR)/$(DIR)/files-tgz-bin.txt pret" ; \
	else \
		echo "     $(DIR)/$(DIR)/files-tgz-bin.txt generated" ; \
	fi ;
endif

# Building the list of files to include in the devel package when called by
# layer 2 Makefile, or devel distribution when called by layer 1 Makefile.
# Depend on the local configuration in Makefile-var.
# NB: including obj files in icons subdirectory (icon source files with tgif)
# NB: search for subdirectory src* containing Makefile...
files-tgz-dev:
ifeq ($(strip $(TGZ_DEV)),)
	@if [ $(TEXT) = "french" ]; then \
		echo "  ++ $(DIRNAME) : regle $@" ; \
		echo "*    sans effet ici" ; \
	else \
		echo "  ++ $(DIRNAME) : $@ rule" ; \
		echo "*    no effect here" ; \
	fi ;
else
	@if [ $(TEXT) = "french" ]; then \
		echo "  ++ $(DIRNAME) : regle $@" ; \
	else \
		echo "  ++ $(DIRNAME) : $@ rule" ; \
	fi ; \
	if [ -e ./files-tgz-dev.txt ] ; then \
		rm ./files-tgz-dev.txt ; \
	fi ; \
	for F in $(filter-out $(FILTER_OUT),$(shell ls $(TGZ_DEV) 2> /dev/null)) icons/*obj ; do \
		if [ -e $$F ]; then \
			echo $$F >> ./files-tgz-dev.txt ; \
		else \
			if [ $(TEXT) = "french" ]; then \
				echo "*    $(DIRNAME) : $$F manquant" ; \
			else \
				echo "*    $(DIRNAME) : $$F not found" ; \
			fi ; \
		fi ; \
	done ; \
	for R in $(shell ls -d src* 2> /dev/null); do \
	  if [ -d $$R ]; then \
			if [ -e $$R/Makefile ]; then \
				echo $$R >> ./files-tgz-dev.txt ; \
				$(MAKE) $@ $(MFLAGS) -C $$R ; \
				for F in `cat $$R/files-tgz-dev.txt 2> /dev/null` ; do \
					echo $$R/$$F >> ./files-tgz-dev.txt ; \
				done ; \
		  else \
				if [ $(TEXT) = "french" ]; then \
					echo "*    repertoire $$R sans Makefile => ignore" ; \
				else \
					echo "*    directory $$R without Makefile => ignored" ; \
				fi ; \
			fi ; \
		fi ; \
	done ; \
	if [ $(TEXT) = "french" ]; then \
		echo "     $(DIR)/$(DIR)/files-tgz-dev.txt pret" ; \
	else \
		echo "     $(DIR)/$(DIR)/files-tgz-dev.txt generated" ; \
	fi ;
endif

# Building the list of files to include in the public package when called by
# layer 2 Makefile, or public distribution when called by layer 1 Makefile.
# Depend on the local configuration in Makefile-var.
# NB: search for subdirectory src* containing Makefile...
files-tgz-pub:
ifeq ($(strip $(TGZ_PUB)),)
	@if [ $(TEXT) = "french" ]; then \
		echo "  ++ $(DIRNAME) : regle $@" ; \
		echo "*    sans effet ici" ; \
	else \
		echo "  ++ $(DIRNAME) : $@ rule" ; \
		echo "*    no effect here" ; \
	fi ;
else
	@if [ $(TEXT) = "french" ]; then \
		echo "  ++ $(DIRNAME) : regle $@" ; \
	else \
		echo "  ++ $(DIRNAME) : $@ rule" ; \
	fi ; \
	if [ -e ./files-tgz-pub.txt ] ; then \
		rm ./files-tgz-pub.txt ; \
	fi ; \
	for F in $(filter-out $(FILTER_OUT),$(shell ls $(TGZ_PUB) 2> /dev/null)) ; do \
		if [ -e $$F ]; then \
			echo $$F >> ./files-tgz-pub.txt ; \
		else \
			if [ $(TEXT) = "french" ]; then \
				echo "*    $(DIRNAME) : $$F manquant" ; \
			else \
				echo "*    $(DIRNAME) : $$F not found" ; \
			fi ; \
		fi ; \
	done ; \
	for R in $(shell ls src* 2> /dev/null); do \
	  if [ -d $$R ]; then \
			if [ -e $$R/Makefile ]; then \
				echo $$R >> ./files-tgz-pub.txt ; \
				$(MAKE) $@ $(MFLAGS) -C $$R ; \
				for F in `cat $$R/files-tgz-pub.txt 2> /dev/null` ; do \
					echo $$R/$$F >> ./files-tgz-pub.txt ; \
				done ; \
		  else \
				if [ $(TEXT) = "french" ]; then \
					echo "*    repertoire $$R sans Makefile => ignore" ; \
				else \
					echo "*    directory $$R without Makefile => ignored" ; \
				fi ; \
			fi ; \
		fi ; \
	done ; \
	if [ $(TEXT) = "french" ]; then \
		echo "     $(DIR)/$(DIR)/files-tgz-pub.txt pret" ; \
	else \
		echo "     $(DIR)/$(DIR)/files-tgz-pub.txt generated" ; \
	fi ;
endif

# Building the list of files to include in the skeleton package when called by
# layer 2 Makefile, or skeleton distribution when called by layer 1 Makefile.
# Depend on the local configuration in Makefile-var.
# NB: search for subdirectory src* containing Makefile...
files-tgz-skl:
ifeq ($(strip $(TGZ_SKL)),)
	@if [ $(TEXT) = "french" ]; then \
		echo "  ++ $(DIRNAME) : regle $@" ; \
		echo "*    sans effet ici" ; \
	else \
		echo "  ++ $(DIRNAME) : $@ rule" ; \
		echo "*    no effect here" ; \
	fi ;
else
	@if [ $(TEXT) = "french" ]; then \
		echo "  ++ $(DIRNAME) : regle $@" ; \
	else \
		echo "  ++ $(DIRNAME) : $@ rule" ; \
	fi ; \
	if [ -e ./files-tgz-skl.txt ] ; then \
		rm ./files-tgz-skl.txt ; \
	fi ; \
	for F in $(filter-out $(FILTER_OUT),$(shell ls $(TGZ_SKL) 2> /dev/null)) ; do \
		if [ -e $$F ]; then \
			echo $$F >> ./files-tgz-skl.txt ; \
		else \
			if [ $(TEXT) = "french" ]; then \
				echo "*    $(DIRNAME) : $$F manquant" ; \
			else \
				echo "*    $(DIRNAME) : $$F not found" ; \
			fi ; \
		fi ; \
	done ; \
	for R in $(shell ls src* 2> /dev/null); do \
	  if [ -d $$R ]; then \
			if [ -e $$R/Makefile ]; then \
				echo $$R >> ./files-tgz-skl.txt ; \
				$(MAKE) $@ $(MFLAGS) -C $$R ; \
				for F in `cat $$R/files-tgz-skl.txt 2> dev/null` ; do \
					echo $$R/$$F >> ./files-tgz-skl.txt ; \
				done ; \
		  else \
				if [ $(TEXT) = "french" ]; then \
					echo "*    repertoire $$R sans Makefile => ignore" ; \
				else \
					echo "*    directory $$R without Makefile => ignored" ; \
				fi ; \
			fi ; \
		fi ; \
	done ; \
	if [ $(TEXT) = "french" ]; then \
		echo "     $(DIR)/$(DIR)/files-tgz-skl.txt pret" ; \
	else \
		echo "     $(DIR)/$(DIR)/files-tgz-skl.txt generated" ; \
	fi ;
endif

install:
	@if [ $(TEXT) = "french" ]; then \
		echo "  ++ $(DIRNAME) : regle $@" ; \
		echo "     repertoire de la distribution : $(DIR_INSTALL)" ; \
		echo "     repertoire des sources : $(DIR)" ; \
	else \
		echo "  ++ $(DIRNAME) : $@ rule" ; \
		echo "     install directory : $(DIR_INSTALL)" ; \
		echo "     sources directory : $(DIR)" ; \
	fi ;
# bin: installing the executable - TclTk application
# NB: by convention, the executable is not in the bin subdirectory.
	@if [ -e $(OUTPUT).tk ] ; then \
		if [ $(TEXT) = "french" ]; then \
			echo "     lien vers $(OUTPUT).tk dans $(DIR_INSTALL)/bin" ; \
		else \
			echo "     link towards $(OUTPUT).tk in $(DIR_INSTALL)/bin" ; \
		fi ; \
		ln -sf --target-directory $(DIR_INSTALL)/bin ../$(DIR)/$(DIR)/$(OUTPUT).tk ; \
	fi ;
# bin: installing the executable - compiled appplication (architecture dependent)
# NB: a link is added according to the current architecture so that shell
#     scripts relying on the executable does not have to be modified even if
#     several binary cohabit for different architecture.
	@if [ -e bin/$(OUTPUT)-$(ARCH) ] ; then \
		if [ $(TEXT) = "french" ]; then \
			echo "     lien de $(OUTPUT) vers $(OUTPUT)-$(ARCH) dans $(DIR_INSTALL)/bin" ; \
		else \
			echo "     link from $(OUTPUT) to $(OUTPUT)-$(ARCH) in $(DIR_INSTALL)/bin" ; \
		fi ; \
		ln -sf $(OUTPUT)-$(ARCH) $(DIR_INSTALL)/bin/$(OUTPUT) ; \
	fi ;
# bin: installing others executable files from the subdirectory bin
# NB: for compiled application, this includes the executable itself.
# NB: this bin subdirectory contains companion applications or shell scripts as
#     well as scripts for launching demo.
	@if [ -e bin ] ; then \
		if [ $(TEXT) = "french" ]; then \
			echo "     lien vers les fichiers de bin dans $(DIR_INSTALL)/bin" ; \
		else \
			echo "     link towards bin files in $(DIR_INSTALL)/bin" ; \
		fi ; \
		for F in $(filter-out $(FILTER_OUT),$(shell ls bin 2> /dev/null)) ; do \
			echo "       $$F" ; \
			ln -sf --target-directory $(DIR_INSTALL)/bin ../$(DIR)/$(DIR)/bin/$$F ; \
		done ; \
	fi ;
# doc: installing documentation files from the subdirectory doc
	@if [ -e doc ] ; then \
		if [ $(TEXT) = "french" ]; then \
			echo "     lien vers les fichiers de doc dans $(DIR_INSTALL)/doc" ; \
		else \
			echo "     link towards doc files in $(DIR_INSTALL)/doc" ; \
		fi ; \
		for F in $(filter-out $(FILTER_OUT),$(shell ls doc 2> /dev/null)) ; do \
			echo "       $$F" ; \
			ln -sf --target-directory $(DIR_INSTALL)/doc ../$(DIR)/$(DIR)/doc/$$F ; \
		done ; \
	fi ;
# icons: Appicon for Window Maker in GNUstep directory
	@if [ -e ~/GNUstep/Library/Icons ] ; then \
		if [ $(TEXT) = "french" ]; then \
			echo "     lien vers icons/apg-icon-$(OUTPUT).ppm dans ~/GNUstep/Library/Icons (Window Maker)" ; \
		else \
			echo "     link towards icons/apg-icon-$(OUTPUT).ppm in ~/GNUstep/Library/Icons (Window Maker)" ; \
		fi ; \
		ln -sf --target-directory ~/GNUstep/Library/Icons `pwd`/icons/apg-icon-$(OUTPUT).ppm ; \
	fi ;
# icons: installing other images from the icons subdirectory
	@if [ -e icons ] ; then \
		if [ $(TEXT) = "french" ]; then \
			echo "     lien vers les fichiers de icons dans $(DIR_INSTALL)/icons" ; \
		else \
			echo "     link towards icons files in $(DIR_INSTALL)/icons" ; \
		fi ; \
		for F in $(filter-out $(FILTER_OUT),$(shell ls icons 2> /dev/null)) ; do \
			echo "       $$F" ; \
			ln -sf --target-directory $(DIR_INSTALL)/icons ../$(DIR)/$(DIR)/icons/$$F ; \
		done ; \
	fi ;
# input: creating the subdirectory for the application
	@if [ $(TEXT) = "french" ]; then \
		echo "     creation du sous-repertoire $(OUTPUT) dans $(DIR_INSTALL)/input (si absent)" ; \
	else \
		echo "     creating subdirectory $(OUTPUT) in $(DIR_INSTALL)/input (if not existing)" ; \
	fi ; \
	mkdir -p $(DIR_INSTALL)/input/$(OUTPUT)
# input: installing any file from the input subdirectory
	@if [ -e input ] ; then \
		if [ $(TEXT) = "french" ]; then \
			echo "     lien vers fichiers de input dans $(DIR_INSTALL)/input/$(OUTPUT)" ; \
		else \
			echo "     link towards input files in $(DIR_INSTALL)/input/$(OUTPUT)" ; \
		fi ; \
		for F in $(filter-out $(FILTER_OUT),$(shell ls input 2> /dev/null)) ; do \
			echo "       $$F" ; \
			ln -sf --target-directory $(DIR_INSTALL)/input/$(OUTPUT) ../../$(DIR)/$(DIR)/input/$$F ; \
		done ; \
	fi ;
# LIBAPGTK: installing the TclTk application library if present
# NB: most of TclTk Airplug applications provide libraries to other applications
#     to provide mnemonics and related variables to use (and sometimes
#     procedures to call) to communicate with them.
	@for F in $(shell ls libapg-app-$(OUTPUT)*.tk 2> /dev/null) ; do \
		if [ $(TEXT) = "french" ]; then \
			echo "     lien vers $$F dans $(DIR_INSTALL)/LIBAPGTK/LIBAPGTK" ; \
		else \
			echo "     link towards $$F in $(DIR_INSTALL)/LIBAPGTK/LIBAPGTK" ; \
		fi ; \
		ln -sf --target-directory $(DIR_INSTALL)/LIBAPGTK/LIBAPGTK ../../$(DIR)/$(DIR)/$$F ; \
	done ;
# LIBAPGTK: installing the TclTk Airplug libraries if present
# NB: some Airplug applications complete the Airplug libapgtk itself:
#     so called 'Airplug hen' adding a new mode for using Airplug applications.
#     Such libraries are named without 'app' to distinguish them.
	@if [ -e libapg-$(OUTPUT).tk ] ; then \
		if [ $(TEXT) = "french" ]; then \
			echo "     lien vers libapg-$(OUTPUT).tk dans $(DIR_INSTALL)/LIBAPGTK/LIBAPGTK" ; \
		else \
			echo "     link towards libapg-$(OUTPUT).tk in $(DIR_INSTALL)/LIBAPGTK/LIBAPGTK" ; \
		fi ; \
		ln -sf --target-directory $(DIR_INSTALL)/LIBAPGTK/LIBAPGTK ../../$(DIR)/$(DIR)/libapg-$(OUTPUT).tk ; \
	fi ;
# output: creating the subdirectory for the application
	@if [ $(TEXT) = "french" ]; then \
		echo "     creation du sous-repertoire $(OUTPUT) dans $(DIR_INSTALL)/output (si absent)" ; \
	else \
		echo "     creating subdirectory $(OUTPUT) in $(DIR_INSTALL)/output (if not existing)" ; \
	fi ; \
	mkdir -p $(DIR_INSTALL)/output/$(OUTPUT)


### DISTRIB MANAGEMENT RULES ###################################################
icon: 
	@if [ $(TEXT) = "french" ]; then \
		echo "  ++ $(DIRNAME) : regle $@" ; \
		echo "     creation de icons/apg-icon-$(OUTPUT).ppm et icons/apg-appicon-$(OUTPUT).ppm" ; \
	else \
		echo "  ++ $(DIRNAME) : $@ rule" ; \
		echo "     creating icons/apg-icon-$(OUTPUT).ppm and icons/apg-appicon-$(OUTPUT).ppm" ; \
	fi ; \
	sed -e "s/APP/$(DIR)/g" < $(DIR_INSTALL)/icons/apg-icon-default.obj > icons/apg-icon-$(OUTPUT).obj ; \
	sed -e "s/APP/$(DIR)/g" < $(DIR_INSTALL)/icons/apg-appicon-default.obj > icons/apg-appicon-$(OUTPUT).obj ; \
	if [ "x`which $(TGIF)`" = "x" ]; then \
		if [ $(TEXT) = "french" ]; then \
				echo "-    $(TGIF) non trouve. Verifier que ce logiciel est installe." ; \
		else \
				echo "-    $(TGIF) not found. Check that this software is installed." ; \
		fi ; \
	else \
		$(TGIF) -print -ppm -o./icons icons/apg-icon-$(OUTPUT).obj 2> /dev/null ; \
		$(TGIF) -print -ppm -o./icons icons/apg-appicon-$(OUTPUT).obj 2> /dev/null ; \
		$(CONVERT) -resize 64x64 icons/apg-icon-$(OUTPUT).ppm icons/apg-icon-$(OUTPUT).ppm ; \
		$(CONVERT) -resize 64x64 icons/apg-appicon-$(OUTPUT).ppm icons/apg-appicon-$(OUTPUT).ppm ; \
	fi

# List files
list:
ifeq ($(strip $(TGZ_BIN)),)
	@if [ $(TEXT) = "french" ]; then \
		echo "  ++ $(DIRNAME) : regle $@" ; \
	else \
		echo "  ++ $(DIRNAME) : $@ rule" ; \
	fi ; \
	echo "     ${BU}Architecture${EU} : any (not a compiled program)" ; \
	echo "     ${BU}Sources${EU} : $(SOURCES)" ; \
	echo "     ${BU}Lib${EU} : $(LIB)" ; \
	for F in $(SOURCES) ; do \
		if ! [ -e $$F ]; then \
			if [ $(TEXT) = "french" ]; then \
				echo "*    $(DIRNAME) : $$F manquant" ; \
			else \
				echo "*    $(DIRNAME) : $$F not found" ; \
			fi ; \
		fi ; \
	done ;
else
	@if [ $(TEXT) = "french" ]; then \
		echo "  ++ $(DIRNAME) : regle $@" ; \
		echo "     ${BU}Architecture${EU} : $(ARCH)" ; \
		echo "     ${BU}Fichiers .c${EU}  : $(C_FILES)" ; \
		echo "     ${BU}Fichiers .h${EU}  : $(H_FILES)" ; \
		echo "     ${BU}Fichier rc${EU}   : $(RC_FILE)" ; \
		echo "     ${BU}Bibliotheque${EU} : $(L_FILES)" ; \
		echo "     ${BU}Fichiers .o${EU}  : $(O_FILES:obj-$(ARCH)/%.o=%.o)" ; \
		echo "        ${BU}existants${EU} : $(shell cd obj-$(ARCH) 2> /dev/null && ls $(OUTPUT)*.o 2> /dev/null)" ; \
		echo -n "     ${BU}Executable${EU}   : " ; \
		if [ -f "bin/$(OUTPUT)-$(ARCH)" ]; then \
		  	echo "bin/$(OUTPUT)-$(ARCH)" ; \
	  else \
	      echo "absent" ; \
    fi ; \
	else \
		echo "  ++ $(DIRNAME) : $@ rule" ; \
		echo "     ${BU}Architecture${EU} : $(ARCH)" ; \
		echo "     ${BU}Files .c${EU}     : $(C_FILES)" ; \
		echo "     ${BU}Files .h${EU}     : $(H_FILES)" ; \
		echo "     ${BU}File rc${EU}      : $(RC_FILE)" ; \
		echo "     ${BU}Library${EU}      : $(L_FILES)" ; \
		echo "     ${BU}Files .o${EU}     : $(O_FILES:obj-$(ARCH)/%.o=%.o)" ; \
		echo "     ${BU}Existing .o${EU}  : $(shell cd obj-$(ARCH) 2> /dev/null && ls $(OUTPUT)*.o 2> /dev/null)" ; \
		echo -n "     ${BU}Program${EU}      : " ; \
		if [ -f "bin/$(OUTPUT)-$(ARCH)" ]; then \
		  	echo "bin/$(OUTPUT)-$(ARCH)" ; \
	  else \
	      echo "lacking" ; \
    fi ; \
	fi ; 
endif

# Used by the newapp.sh shell script
print-tgz-dev:
	@echo "$(TGZ_DEV)"


###  COMPILATION RULES #########################################################
# NB: any compilation rule (specific to the application) are defined in
#     Makefile-bin to keep a generic Makefile.
include Makefile-bin

