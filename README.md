# _Verdi_
sandbox for JTAG and ATPG SCAN chains

track from RTL internal signals 
register files implemented as latch arrays 
controlled via registers per cluster/region
<chip>_<rev>_scan_<clk>_<patType>_<pkg>_<rev>
Call chain: Full → base_lwbist_interface → flush0/flush1 → read_lw_bist_status_chain
Writes known data patterns (flush0, flush1) into latch arrays; reads back; computes CRC; compares against golden signature (<cluster>_scan.signature.tcl)
generate_lw_bist_signature.pl -domains_pm domains_rtl.pm -iteration 1 -input_config_yaml ramgen.yml
lw_bist_en → flush_0/flush_1 → lw_bist_vld → waitRti → write → read → status chain shift-out
CRC on RAM_ACCESS_DATA_lw_bist_sts matches expected signature
