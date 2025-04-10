# Actuator

Brotherlogic
Apr 10, 2025

## Abstract

The MDB actuator is the piece of MDB that actuates the link between machines and the existing MDB database.

## Features

MDB maintains a database of the state of machines in the network, we then use the database to push ansible changes to those machines. Given the state of the network, we should be able to reach all the machines that we need to.

## Process

Define a ruleset for reachable machines - we use the MACHINE_USE setting to configure which machine uses need to be reachable. The actuator is then able to run an ansible script to validate machine reachability (individually). An unreachable machine is flagged ana bug raised to investigate and fix. The bug is then closed when we are able to reach the machine. A reachable machine is flagged in the MDB as such, with the reached time. We validate reachability every hour. A machine which is unreachable for 24 hours causes a bug to be raised.

## Tasks

1. Actuator runs in argon5
1. Actuator pulls config from pstore on startup
1. Ansible check on reachable works for test machine
1. Actuator pulls machine IPs from MDB
1. Runs reachable test on each IP
1. On reachable -> update MDB with last reached time, close any open bugs
1. On unreachable -> check mdb for last reached time, raise issue if > 24 hours
1. Actuator runs every 60 minutes
1. Actuator pushes stats to prometheus on run end
1. Dashboard created for displaying reachability metrics
