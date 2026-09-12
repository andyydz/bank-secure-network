# Documentation

This folder contains the technical documentation for the **Bank Secure Network** project.

## Contents

### Network Design

Contains information about the physical and logical network architecture, including:

- Network topology
- Device roles
- Switch connections
- Router configuration
- Network segmentation
- Traffic flow

### IP & VLAN Plan

Contains the IP addressing and VLAN allocation used throughout the network.

## VLAN Structure

| VLAN | Name | Network | Gateway |
|------|------|---------|---------|
| 10 | ADMIN | 192.168.10.0/24 | 192.168.10.1 |
| 20 | EMPLOYEE | 192.168.20.0/24 | 192.168.20.1 |
| 30 | ATM | 192.168.30.0/24 | 192.168.30.1 |
| 40 | SERVER | 192.168.40.0/24 | 192.168.40.1 |

## Purpose

The documentation explains how the network was designed, configured, secured, and tested.

Additional documentation may be added as the project progresses.
