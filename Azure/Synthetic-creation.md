# Synthetic Monitoring

## Create Browser Monitor

Dynatrace

→ Observe and Explore

→ Synthetic Monitoring

→ Create Monitor

## Configuration

Monitor Type:

Browser Monitor

Monitor Name:

EasyTrade Availability

Frequency:

Every 5 Minutes

Target:

EasyTrade Frontend URL

## Validation

Run Monitor

Expected Result:

Passed

## Failure Simulation

kubectl scale deployment easytrade-frontendreverseproxy \
-n easytrade \
--replicas=0

## Expected Result

Synthetic Failure

↓

Davis AI Problem

↓

Root Cause Analysis
