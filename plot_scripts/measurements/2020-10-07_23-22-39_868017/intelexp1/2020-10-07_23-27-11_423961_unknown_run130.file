#!/bin/bash

# Script is run locally on experiment server.

# exit on error
set -e
# log every command
set -x

MOONGEN_DIR=$(atm_get_variable moongen_dir --from-global)
PKT_RATE=$(atm_get_variable pkt_rate --from-loop)
PKT_SZ=$(atm_get_variable pkt_sz --from-loop)

LOADGEN_INGRESS_DEV=$(atm_get_variable --from-global loadgen_ingress_dev)
LOADGEN_EGRESS_DEV=$(atm_get_variable --from-global loadgen_egress_dev)
LOADGEN_INGRESS_IF=$(atm_get_variable --from-global loadgen_ingress_if)
LOADGEN_EGRESS_IF=$(atm_get_variable --from-global loadgen_egress_if)
LOADGEN_INGRESS_MAC=$(atm_get_variable --from-global loadgen_ingress_mac)
LOADGEN_EGRESS_MAC=$(atm_get_variable --from-global loadgen_egress_mac)
LOADGEN_INGRESS_IP=$(atm_get_variable --from-global loadgen_ingress_ip)
LOADGEN_EGRESS_IP=$(atm_get_variable --from-global loadgen_egress_ip)
LOADGEN_ENABLE_IP_SW_CHKSUM_CALC=$(atm_get_variable --from-global loadgen_enable_ip_sw_chksum_calc)

LOADGEN_WARM_UP=$(atm_get_variable warm_up)

DUT_INGRESS_MAC=$(atm_get_variable --from-global dut_ingress_mac)
DUT_EGRESS_MAC=$(atm_get_variable --from-global dut_egress_mac)
DUT_INGRESS_IP=$(atm_get_variable --from-global dut_ingress_ip)
DUT_EGRESS_IP=$(atm_get_variable --from-global dut_egress_ip)

PKTS_TOTAL=$(($PKT_RATE*30))

echo "send packets with size: $PKT_SZ and rate: $PKT_RATE."

atm_sync

atm_run --loop loadgen -- bash -c "$MOONGEN_DIR/build/MoonGen $MOONGEN_DIR/examples/soft-gen.lua --src-mac $LOADGEN_EGRESS_MAC --dst-mac $DUT_INGRESS_MAC --src-ip $LOADGEN_EGRESS_IP --dst-ip $LOADGEN_INGRESS_IP --fix-packetrate $PKT_RATE --size $PKT_SZ --packets $PKTS_TOTAL --ip-chksum $LOADGEN_ENABLE_IP_SW_CHKSUM_CALC --warm-up $LOADGEN_WARM_UP $LOADGEN_EGRESS_DEV $LOADGEN_INGRESS_DEV > /root/throughput.log"

sleep 50

atm_kill --loop loadgen

sleep 100

atm_upload --loop /root/throughput.log

sleep 5

atm_sync

echo "experiment successful"
