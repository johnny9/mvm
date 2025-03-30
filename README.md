# MVM: Miner Vending Machine Overview

**MVM (Miner Vending Machine)** is a system for managing and automating tests on hardware devices (e.g., miners). It provides:
- A device reservation and management service (the “vending machine”).
- Automated workflows for flashing, configuring, and testing devices.
- Integration points with your CI pipeline or scheduler.

## Table of Contents

1. [System Architecture](#system-architecture)
2. [Key Components](#key-components)
   - [MVM Reservation Service](./mvm-reservation-service.md)
   - [Automation & Test Orchestrator](./orchestrator.md)
   - [Test Framework & Flashing Service](./test-framework.md)
   - [CI/Build System Integration](./ci-integration.md)
3. [Test Script Definition](#test-script-definition)
4. [Workflow Overview](#workflow-overview)
5. [API Reference](#api-reference)
6. [Getting Started](#getting-started)
7. [Contributing](#contributing)

---

## System Architecture

This platform is composed of four main pieces:

1. **MVM Reservation Service**  
   - Maintains the database of miners (devices).
   - Exposes REST APIs for searching, reserving, releasing, and decommissioning devices.
   - Ensures only one user or process has exclusive access (locked state) to each device at a time.

2. **Automation & Test Orchestrator**  
   - Handles scheduling of test jobs and coordinating device reservations.
   - Integrates with MVM to reserve and release devices.
   - Kicks off flashing and test execution, then collects results.

3. **Test Framework & Flashing Service**  
   - Contains the lower-level logic for flashing firmware/software onto devices.
   - Executes test suites, verifies results, and updates device status.

4. **CI/Build System**  
   - Your existing CI platform (Jenkins, GitHub Actions, etc.).
   - Triggers the Orchestrator automatically on code commits or on a schedule.

---

## Key Components

### MVM Reservation Service
This service acts as the “inventory” or “vending machine” for your miners:
- Tracks devices’ availability, hardware type, location, and state.
- Locks devices for a test session, preventing collisions.
- Provides endpoints for releasing devices or changing device states.

Read the full documentation:  
[**MVM Reservation Service**](./mvm-reservation-service.md)

---

### Automation & Test Orchestrator
Coordinates the full lifecycle of a test run:
- Reserves devices through MVM (based on hardware, software, and attributes).
- Calls the Flashing Service to update firmware/config.
- Executes the actual tests in the Test Framework.
- Releases devices when tests complete.

Learn more:  
[**Automation & Test Orchestrator**](./orchestrator.md)

---

### Test Framework & Flashing Service
Defines the test logic and device flashing processes:
- Standard routines for firmware updates and configurations.
- Automated test scripts for various functional and performance scenarios.
- Mechanisms to capture logs, results, and error states.

More details here:  
[**Test Framework & Flashing Service**](./test-framework.md)

---

### CI/Build System Integration
Illustrates how MVM integrates with your existing CI processes:
- Automatic test runs triggered on code pushes or merges.
- Nightly or scheduled runs for regression testing.
- Best practices for reporting results back to developers.

Read this section:  
[**CI/Build System Integration**](./ci-integration.md)

---

## Test Script Definition

Tests are driven by a simple configuration file (in JSON or YAML) that specifies:
1. **Which script** to run (e.g., a Python test file).
2. **What devices** are required (hardware, software, attributes).

Below is an example in JSON form:

```json
{
  "script": "basic_.py",
  "devices": [
    {
      "hardware": "401",
      "software": "esp-miner-2.6.0",
      "attributes": []
    }
  ]
}
