# Makefile contributed by Alain Frisch

# the list is topologically sorted
MODULES := \
 extBytes enum bitSet dynArray extArray extHashtbl extList extString extBuffer \
 global IO option pMap std uChar uTF8 base64 unzip refList optParse dllist extLib

ifdef minimal
MODULES := $(filter-out unzip uChar uTF8, $(MODULES))
endif

OCAMLC = ocamlfind ocamlc -g -package bytes
OCAMLOPT = ocamlfind ocamlopt -g -package bytes

OUT = extLib.mli extBytes.mli extList.mli

MLI = $(filter-out $(OUT), $(MODULES:=.mli))
CMI = $(MODULES:=.cmi)
CMO = $(MODULES:=.cmo)
CMX = $(MODULES:=.cmx)

.NOTPARALLEL:
.SUFFIXES:
.PHONY: build all opt cmxs doc install uninstall clean release

build: all opt cmxs

all: extLib.cma
opt: extLib.cmxa
cmxs: extLib.cmxs

doc:
	$(OCAMLC) -c $(MLI)
	ocamldoc -sort -html -d doc/ $(MLI) extLib.ml


extBytes.mli: extBytes.ml
	$(OCAMLC) -i  > $@
	rm $@
extBytes.cmxs: extBytes.cmxa
	$(OCAMLOPT) -shared -linkall -o $@
extBytes.cmo: extBytes.mli extBytes.ml
	$(OCAMLC) -c extBytes.ml
extBytes.cmx: extBytes.mli extBytes.ml
	$(OCAMLOPT) -c extBytes.ml


extLib.cma: $(CMO)
	$(OCAMLC) -a -o $@ $^
extLib.cmxa: $(CMX)
	$(OCAMLOPT) -a -o $@ $^
%.cmxs: %.cmxa
	$(OCAMLOPT) -shared -linkall $< -o $@
%.cmo: %.mli %.ml
	$(OCAMLC) -c $^
%.cmx: %.mli %.ml
	$(OCAMLOPT) -c $^
extLib.cmo: extLib.ml
	$(OCAMLC) -c $<
extLib.cmx: extLib.ml
	$(OCAMLOPT) -c $<



%.ml: %.mlpp
	cppo $(shell ocaml configure.ml) $< -o $@

%.mli: %.mlipp
	cppo $(shell ocaml configure.ml) $< -o $@

install:
	ocamlfind install -patch-version $(VERSION) extlib META extLib.cma $(MLI) $(CMI) -optional extLib.cmxa $(CMX) extLib.cmxs extLib.a extLib.lib

uninstall:
	ocamlfind remove extlib

clean:
	rm -f *.cmo *.cmx *.o *.obj *.cmi *.cma *.cmxa *.cmxs *.a *.lib doc/*.html
	rm -rf extHashtbl.ml extBuffer.ml extBytes.ml extBytes.mli extString.ml
