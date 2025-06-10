# A2:201 - Cryptographic Failures

## Overview
[Cryptographic Failures](https://owasp.org/Top10/A02_2021-Cryptographic_Failures/) occur when applications fail to properly protect data in transit and at rest through inadequate or missing cryptographic controls. This vulnerability category encompasses issues such as using weak encryption algorithms, improper key management, transmitting sensitive data in plaintext, and failing to encrypt sensitive data altogether.

Cryptographic failures can lead to exposure of sensitive data including personal information, authentication credentials, financial data, and other confidential information. These failures often result from using outdated cryptographic standards, poor implementation of encryption, or complete absence of encryption where it should be applied.

## Reconnaissance
This vulnerability as discovered during the [injection](./03-injection.md) of the walk through. Do that one first.