# External REST Function — Execute OTBI Report

## Overview

This external REST function executes an OTBI (Oracle Transactional Business Intelligence) report using the Oracle BI SOAP Web Service `executeXMLQuery` operation.

The function requires a valid Session ID generated from the authentication service.

---

## Configuration

| Property | Value |
|---|---|
| Function Name | `ExecuteOTBIReport` |
| Operation Type | `HTTP POST` |
| Resource Path | `/analytics-ws/saw.dll/wsdl/v12` |
| Description | Execute OTBI Report using SOAP Web Service |

---

## Request Headers

```http
Content-Type: application/soap+xml
```

---

## Request Body Template

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:v6="urn://oracle.bi.webservices/v6">
   <soapenv:Header/>
   <soapenv:Body>
      <v6:executeXMLQuery>
         <v6:report>
            <v6:reportPath>{reportpath}</v6:reportPath>
            <v6:reportXml>?</v6:reportXml>
         </v6:report>
         <v6:outputFormat>?</v6:outputFormat>
         <v6:executionOptions>
            <v6:async>?</v6:async>
            <v6:maxRowsPerPage>?</v6:maxRowsPerPage>
            <v6:refresh>?</v6:refresh>
            <v6:presentationInfo>?</v6:presentationInfo>
            <v6:type>?</v6:type>
         </v6:executionOptions>
         <v6:sessionID>{sessionid}</v6:sessionID>
      </v6:executeXMLQuery>
   </soapenv:Body>
</soapenv:Envelope>
```

---

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `reportPath` | `string` | Yes | Full OTBI report path |
| `sessionid` | `string` | Yes | Valid Oracle Analytics session ID |

---

## Notes

- A valid Session ID is mandatory.
- The report path must exactly match the OTBI catalog path.
- This service uses SOAP over HTTP POST with Oracle Fusion AI Agent Studio.
- Ensure the SOAP namespace is preserved exactly as provided.
