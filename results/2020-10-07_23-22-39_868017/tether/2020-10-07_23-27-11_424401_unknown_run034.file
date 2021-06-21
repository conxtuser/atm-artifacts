#!/bin/bash

# Script is run locally on experiment server.

# exit on error
set -e
# # log every command
set -x

PKT_RATE=$(atm_get_variable pkt_rate --from-loop)
PKT_SZ=$(atm_get_variable pkt_sz --from-loop)

echo "forward packets with size: $PKT_SZ and rate: $PKT_RATE."

atm_sync

sleep 1

atm_sync

echo "experiment successful"
