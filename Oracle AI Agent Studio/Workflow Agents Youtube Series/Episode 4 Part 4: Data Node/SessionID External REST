# External REST Function — Fetch Oracle Analytics OTBI Session ID

## Overview

This external REST function authenticates against Oracle Analytics SOAP Web Services and returns a Session ID using the `logon` operation.

---

## Configuration

| Property | Value |
|---|---|
| Function Name | `SessionID` |
| Operation Type | `HTTP POST` |
| Resource Path | `/analytics-ws/saw.dll/wsdl/v12` |
| Description | Fetch Oracle OTBI Session ID |

---

## Request Headers

```http
Content-Type:application/soap+xml
```

---

## Request Body Template

```xml
<soapenv:Envelope xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:v6="urn://oracle.bi.webservices/v6">
   <soapenv:Header/>
   <soapenv:Body>
      <v6:logon>
         <v6:name>{username}</v6:name>
         <v6:password>{password}</v6:password>
      </v6:logon>
   </soapenv:Body>
</soapenv:Envelope>
```

---

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `username` | `string` | Yes | Oracle Analytics / Fusion username |
| `password` | `string` | Yes | Oracle Analytics / Fusion password |

---

## Notes

- Ensure SOAP namespaces are preserved exactly as provided.
- The returned Session ID can be used for subsequent SOAP-based report execution calls.
- Valid Oracle Analytics or Fusion credentials are required.
- This service uses SOAP over HTTP POST within Oracle Fusion AI Agent Studio.
- If needed, username and password can also be harcoded.
