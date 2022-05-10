---
title: Dev AANew Call a remote function module
description: Call a remote function module located in an on-premise system
auto_validation: true
time: 45
tags: [tutorial>how-to, tutorial>beginner, tutorial>intermediate, programming tool>API]
primary_tag: topic>User-Interface
---

## Prerequisites
-	A full entitlement to [SAP Cloud Platform, ABAP environment](https://cloudplatform.sap.com/capabilities/product-info.SAP-Cloud-Platform-ABAP-environment.4d0a6f95-42aa-4157-9932-d6014a68d825.html) (not a trial account)
- A full SAP Cloud Platform Neo subaccount. **IMPORTANT**: Your SAP Cloud Platform, Cloud Foundry and SAP Cloud Platform, Neo accounts must be in the same geographical region.
-	An ABAP on-premise system, such as:
    - [SAP S/4HANA 1809 fully activated appliance](https://blogs.sap.com/2018/12/12/sap-s4hana-fully-activated-appliance-create-your-sap-s4hana-1809-system-in-a-fraction-of-the-usual-setup-time/) or:
    - [The SAP Gateway Demo System (ES5)](https://blogs.sap.com/2017/12/05/new-sap-gateway-demo-system-available/)
-	In this on-premise system, a SAP Cloud Connector

## Details
### You will learn
  - How to open a secure tunnel connection between your SAP Cloud Platform ABAP Environment and an on-premise SAP System, e.g. SAP S/4HANA
  - How to create a destination service instance with HTTP and RFC connections
  - How to create a communication arrangement to integrate this destination service
  - How to create a communication arrangement to integrate SAP Cloud Connector
  - How to test the connection using an ABAP handler class
  - How to create a communication arrangement to integrate SAP Cloud Connector
  - How to test the connection using an ABAP handler class

Throughout this tutorial, replace `XXX` with your initials or group number.

**The problem:**

There are two problems when setting up connectivity between the Cloud Platform ABAP Environment and an on-premise:

- The ABAP Environment "lives" in the Internet, but customer on-premise systems are behind a firewall
- RFC is not internet-enabled

**The solution**:

- Set up a connection from the on-premise system to the SAP Cloud Platform Neo Environment using SAP Cloud Connector
- Set up a connection from the SAP Neo to the SAP Cloud Foundry Environment

**Specifically**:

1. Fetch the destination, i.e. from SAP Cloud Foundry to on-premise  (using a Cloud Foundry destination service)
2. Send request to open a tunnel, from Cloud Foundry (i.e. ABAP Environment) to SAP Neo
3. Send request to open a tunnel, from Neo to the on-premise system
4. Open a secure tunnel for HTTP and RFC
5. Communicate through the tunnel via HTTP or RFC

![Image depicting overview](overview.png)

---

[ACCORDION-BEGIN [Step 1: ](Configure SAP Cloud Connector)]
First, you need to connect your ABAP on-premise system to a Neo subaccount by means of SAP Cloud Connector.

1. Log on to SAP Cloud Connector:
    - Address = e.g. `https://localhost:<port>` (Default = 8443)
    - User = Administrator
    - Initial password = Manage (You will change this when you first log in)

2. Choose **Add Subaccount**:
  - **Region** = Your region. You can find this in SAP Cloud Cockpit (see screenshot below). Note that your SAP Cloud Platform Neo and SAP Cloud Foundry accounts need to run in the same region (e.g. here, Europe Rot)
  - **Subaccount** = "Neo Technical Name". You can find this by choosing your Neo subaccount in SAP Cloud Cockpit and choosing the **information (i)** icon. (see screenshot below)
  - **Display Name** = (Neo Subaccount) Display Name. You can find this in by choosing your Neo subaccount in SAP Cloud Cockpit (see screenshot below)
  - **Subaccount User** = for the Neo Subaccount
  - **Password**
  - **Location ID** = Optional here. However, it is mandatory if you want to connect several Cloud Connectors to your subaccount. This can be any text, e.g. your initials

  !![Image depicting step1g-create-subacc](step1g-create-subacc.png)
  !![Image depicting step1f-choose-subacc](step1f-choose-subacc.png)
  !![Image depicting step1b-neo-tech-name ](step1b-neo-tech-name.png)

Your configuration should now look like this:

  ![Image depicting step1-check-scc](step1-check-scc.png)

[DONE]
[ACCORDION-END]

[ACCORDION-BEGIN [Step 2: ](Add On-Premise System)]
1. In your sub-account, **Display Name**, choose **Cloud to On-Premise > Access Control**.

  ![Image depicting step2e-add-onP-system](step2e-add-onP-system.png)

2. In the **Mapping Virtual...** pane, choose **Add (+)**.

  ![Image depicting step2f-add-2](step2f-add-2.png)

3. Enter the following values, and choose **Save**.

    - Backend Type = ABAP
    - Protocol = RFC
    - **Without** load balancing...
    - Internal Host and port (see below)
    - Virtual Host = e.g. `es5host`. Here, `es5host` represents an external hostname, so that you can hide the internal hostname from the outside world. You will need this external hostname and port later.
    - Virtual Port = usually, enter the internal Port
    - Principal Type = None
    - Check Internal Host = Ticked

    ![Image depicting step2g-map-internal-system](step2g-map-internal-system.png)

4. To find the port, in your on-premise system, choose the transaction **`SMICM` > `Goto` > Services**, then choose an active `HTTP` port.

The mapping should now look something like this. Check that the status = `Reachable`. If not, check that you chose the correct port, or whether an internal firewall is preventing communication:

![Image depicting step1a-scc-resources-sid](step1a-scc-resources-sid.png)  

[DONE]
[ACCORDION-END]



