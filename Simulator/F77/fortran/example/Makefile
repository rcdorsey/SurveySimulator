MODELS = InnerHotModel ReadModelFromFile

%: %.sh

test: clean $(MODELS)

$(MODELS):
	$(MAKE) -C .. Driver GIMEOBJ=$@
	@./test.sh ../$@

.PHONY: $(MODELS)

.PHONY: clean

clean:
	\rm -f ModelUsed.dat SimulDetect.dat SimulTrack.dat LOG
