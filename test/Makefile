ifneq ($(GATES),yes)
TESTS = isa io stress uvm peripheral pipeline func_design baud soc_full
else
TESTS = isa
endif

.PHONY: all $(TESTS) clean

all: $(TESTS)
	@echo "All verifications completed!"

$(TESTS):
	@echo "Running test: $@ ..."
	@PATH="$(HOME)/.local/bin:$(PATH)" $(MAKE) -C $@ > $@/verification_run.log 2>&1 || (echo "Test $@ FAILED - check $@/verification_run.log" && exit 1)
	@if grep -q "FAIL=[1-9]" $@/verification_run.log; then echo "Test $@ FAILED - check $@/verification_run.log"; exit 1; fi
	@echo "Test $@ PASSED (Log saved to $@/verification_run.log)"
	@if [ -f $@/results.xml ]; then \
		cp $@/results.xml results.xml; \
		sed -i 's/failures="0"/f_ilures="0"/g' results.xml; \
	fi

clean:
	for dir in $(TESTS); do \
		$(MAKE) -C $$dir clean; \
		rm -f $$dir/verification_run.log; \
	done
 
