# playback

A simple scenarios goes through the new support of DataPower's `SafeNet HSM HA Group` feature. 

## Prerequisites

* At least two SafeNet Network HSMs with Password Authentication.
* HSMs, Partitions,and Crypto Officers, are configured for HA group.
* A Linux workstation with SafeNet Luna HSM Client software installed.
* A DataPower configured as client and registered to use the partitions.
* All the partitions to be included in the HA group has the same keys and certifications.

## Simple Crypto operations

1. Send an HTTP request by curl to run sign then verify actions in CLI.
2. Send an HTTP request by curl to run encrypt then decrypt actions in CLI.

## Scenario 0: Crypto operation via configured Partitions

A normal path using the key/cert from Partitions before HA group configured.

1. Do simple crypto operations.

## Scenario 1: Crypto operation via an HA group

A normal path using the key/cert from HA group.

1. Create an HA group in application domain in Web GUI.
2. Create crypto object configurations which use the key/cert from HA group in Web GUI.
3. Do simple crypto operations.

## Scenario 2: Crypto operation keeps running when one member of HA group is offline

When a member of HA group is offline, client still works as normal.

1. Disconnect the network to one of the SafeNet Network HSMs.
2. Do simple crypto operations.

## Scenario 3: Crypto operation keeps running when the offline member back into HA group

When the offline member back into HA group, client still works as normal.

1. Connections to both SafeNet Network HSMs are OK.
2. Do simple crypto operations.
