# -------------------------------------------------------
#  Copyright (c) 2022-2022 ChipEye All rights reserved
#  -----------------------------------------------------
#  Author	: ATING
#  Filename	: Makefile
#  Date		: 2022-09-20
#  ------------------------------------------------------


# ======Pre preparation==============
# Modify the tb module name
TOP_NAME := vote_7in_tb


.PHONY: all
all: clean add compile wave_gen sim_no_gui run_verdi

.PHONY: shuaxin
shuaxin: compile wave_gen sim_no_gui

.PHONY: add
add:
	@echo "==========================="
	@echo "Step 1 : add file path     "
	@echo "==========================="
	find ../rtl -type f \( -name "*.v" -o -name "*.sv" \) > file.list

.PHONY: compile
compile:
	@echo "==========================="
	@echo "Step 2 : vcs compile       "
	@echo "==========================="
	vcs -full64 -notice -debug_all -j8 -timescale=1ns/1ns \
	+plusarg_save +libext+.v+.V+.sv+.svh -sverilog +memchk \
	-P ${VERDI_HOME}/share/PLI/VCS/LINUX64/novas.tab \
	${VERDI_HOME}/share/PLI/VCS/LINUX64/pli.a \
	+lint=TFIPC-L +nospecify +notimingcheck \
	-f file.list -l com.log

.PHONY: wave_gen
wave_gen:
	@echo "========================================="
	@echo "Step 3 : crete a wave generate tcl script"
	@echo "========================================="
	@if [ -f wave_gen.tcl ]; then rm wave_gen.tcl; fi
	echo 'call {$$fsdbDumpfile("${TOP_NAME}.fsdb")}' >> wave_gen.tcl
	echo 'call {$$fsdbDumpvars(0,${TOP_NAME},"+all")}' >> wave_gen.tcl
	echo 'run' >> wave_gen.tcl
	echo 'q' >> wave_gen.tcl

.PHONY: sim_no_gui
sim_no_gui:
	@echo "==========================="
	@echo "Step 4 : vcs simulation    "
	@echo "==========================="
	./simv -ucli -i wave_gen.tcl -l sim.log

.PHONY: run_verdi
run_verdi:
	@echo "==========================="
	@echo "Step 5 : View waveforms    "
	@echo "==========================="
	verdi -f ./file.list -ssf ${TOP_NAME}.fsdb -nologo &

.PHONY: clean
clean:
	find . ! -name 'Makefile' ! -name 'readme.txt' -type f -exec rm -f {} +
	find . ! -name '.' -type d -exec rm -rf {} +