# _Verdi_
sandbox for JTAG and ATPG SCAN chains

track from RTL internal signals 
register files implemented as latch arrays
Combinational & sequential logic (scan chains)
Pseudo-random (PRPG → scan chains → MISR/CRC)
PRPG (Pseudo-Random Pattern Generator) shifts SCAN patterns into scan chains
MISR/CRC compresses scan-out into a signature 
compared against golden value
Clock Modes:
Runs during DFT boot; 
uses yc_clock for at-speed capture
Bootrecord-configured; 
Master FSM handles init stages, 
NOC transactions, 
clock sequencing
controlled via registers per cluster/region
<chip>_<rev>_scan_<clk>_<patType>_<pkg>_<rev>
Call chain: 
Full → base_JTAG_scan_interface → flush0/flush1 → read_JTAG_scan_status_chain
Writes known data patterns (flush0, flush1) into latch arrays: 
reads back; 
computes CRC; 
compares against golden signature (<cluster>_scan.signature.tcl)
generate_JTAG_scan_signature.pl -domains_pm domains_rtl.pm -iteration 1 -input_config_yaml ramgen.yml
scan_en → flush_0/flush_1 → scan_vld → waitRti → write → read → status chain shift-out
CRC on RAM_ACCESS_DATA_scan matches expected signature
Shift phase → Capture phase (scan enable low, clocks pulse) → signature accumulation
Repeats for N iterations
