# Suricata-IDS
Explore about Suricata alerts and logs, including the general process of rule creation. The Suricata tool monitors network interfaces and applies rules to the packets that pass through the interface.

 # Examine alerts, logs, and rules with Suricata

## Table of Contents
- [Introduction](#introduction)
- [Working with Files](#working-with-files)
- [Examining Rules](#examining-rules) 
- [Running Suricata](#running-suricata)
- [Analyzing Logs](#analyzing-logs)
  - [fast.log](#fastlog)
  - [eve.json](#evejson)

## Introduction

In this lab activity, we’ll explore about Suricata alerts and logs, including the general process of rule creation. The Suricata tool monitors network interfaces and applies rules to the packets that pass through the interface. Suricata determines whether each packet should generate an alert and be dropped, rejected, or allowed to pass through the interface.
Source and destination networks must be specified in the Suricata configuration. Custom rules can be written to specify which traffic should be processed.
We’ll examine a rule and practice using Suricata to trigger alerts on network traffic. We’ll also analyze log outputs, such as a fast.log and eve.json file. This will help us understand some of the alerts and logs generated by Suricata.

## Working with Files

Let’s define the files we’ll be working with in this lab activity from Qwiklabs platform:

- The sample.pcap file is a packet capture file that contains an example of network traffic data, which we’ll use to test the Suricata rules. This will allow us to simulate and repeat the exercise of monitoring network traffic.
- The custom.rules file contains a custom rule when the lab activity starts. We’ll add rules to this file and run them against the network traffic data in the sample.pcap file. 
- The fast.log file will contain the alerts that Suricata generates. The fast.log file is empty when the lab starts. Each time we test a rule, or set of rules, against the sample network traffic data, Suricata adds a new alert line to the fast.log file when all the conditions in any of the rules are met. The fast.log file can be in the /var/log/suricata directory after Suricata runs. The fast.log file is a depreciated format and is not recommended for incident response or threat hunting tasks but can be used to perform quick checks or tasks related to quality assurance.
- The eve.json file is the main, standard, and default log for events generated by Suricata. It contains detailed information about alerts triggered, as well as other network telemetry events, in JSON format. The eve.json file is generated when Suricate runs and can also be in the /var/log/suricata directory.

## Examining Rules

Use the cat command to display the rule in the custom.rules file:

```
cat custom.rules
```
![image](https://github.com/ephrinaw/Suricata-IDS/assets/39829776/7d272501-e35d-405b-8634-22d9404790d1)
List the files in the /var/log/suricata folder: 
```
ls -l /var/log/suricata 
```
![image](https://github.com/ephrinaw/Suricata-IDS/assets/39829776/aec986cb-a89b-497d-8b77-615cbe4d438f)


## Running Suricata

Run suricata using the custom.rules and sample.pcap files:

```
suricata -r sample.pcap -S custom.rules -k none
```
![image](https://github.com/ephrinaw/Suricata-IDS/assets/39829776/ea3e511d-d223-498d-a62a-3989e2dca5b5)

This command starts the Suricata application and processes the sample.pcap file using the rules in the custom.rules file. It returns an output stating how many packets were processed by Suricata.

- The `-r sample.pcap` option specifies an input file to mimic network traffic. In this case, the sample.pcap file.
- The `-S custom.rules` option instructs Suricata to use the rules defined in the custom.rules file. 
- The `-k none` option instructs Suricata to disable all checksum checks.since we are using network traffic from a sample packet capture file, we won't need Suricata to check the integrity of the checksum.

## Analyzing Logs

### fast.log

Use the cat command to display the fast.log file generated by Suricata:

```
cat /var/log/suricata/fast.log
```
![image](https://github.com/ephrinaw/Suricata-IDS/assets/39829776/dff6d87c-5347-4aa8-bc1b-3c5e0b8ed3d3)

### eve.json

Use the cat command to display the entries in the eve.json file:

```
cat /var/log/suricata/eve.json
```
![image](https://github.com/ephrinaw/Suricata-IDS/assets/39829776/601f6bdc-114d-453a-950d-7c133b2ad5ba)

Use the jq command to display the entries in an improved format:

```
jq . /var/log/suricata/eve.json | less
```
![image](https://github.com/ephrinaw/Suricata-IDS/assets/39829776/127dad6b-a631-43da-9184-60794b758073)

Use the jq command to extract specific event data from the eve.json file:

```
jq -c "[.timestamp,flow_id,.alert.signiture,.proto, .dest_ip]" /var/log/suricata/eve.json
```
![image](https://github.com/ephrinaw/Suricata-IDS/assets/39829776/55d36f23-ee5e-42bc-b96d-f593b38fccb8)


