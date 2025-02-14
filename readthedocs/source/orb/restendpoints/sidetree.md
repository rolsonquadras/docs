# Sidetree Endpoints

Endpoints are defined to post Sidetree operations and to resolve DID documents.

## Operations

**Endpoint:** /sidetree/v1/operations

### POST

Post a Sidetree operation as per [Sidetree DID Operations](../sidetree.html#did-operations)

**Example**

Post a _create_ operation:

```json
POST /sidetree/v1/operations HTTP/1.1
Host: orb.domain1.com
Content-Type: application/ld+json

{
  "delta": {
    "patches": [
      {
        "action": "add-public-keys",
        "publicKeys": [
          {
            "id": "createKey",
            "publicKeyJwk": {
              "crv": "P-256",
              "kty": "EC",
              "x": "1p-MlSJlLz03NIzdnYeSRxLCwT5xKAJRA5eqguMB18Q",
              "y": "PXWutlWxQ3xC-60lLZFG5DJYGeMqQTH3TCmc085XYGI"
            },
            "purposes": [
              "authentication"
            ],
            "type": "JsonWebKey2020"
          },
          {
            "id": "auth",
            "publicKeyJwk": {
              "crv": "Ed25519",
              "kty": "OKP",
              "x": "F_0V_LmEz_jePElRfapDw9olzalJ5J2v1n81ZdUedYM",
              "y": ""
            },
            "purposes": [
              "assertionMethod"
            ],
            "type": "Ed25519VerificationKey2018"
          }
        ]
      },
      {
        "action": "add-services",
        "services": [
          {
            "id": "didcomm",
            "priority": 0,
            "recipientKeys": [
              "AksxNrukqJARFPTCkQQNdzkZ6ihXKmsPPx7gS9qpJgzC"
            ],
            "routingKeys": [
              "DiFZ14m43qBkUSTsZXTvSj1B5XBdvRJmGgH6FPVc4mwM"
            ],
            "serviceEndpoint": "https://hub.example.com/.identity/did:example:0123456789abcdef/",
            "type": "did-communication"
          }
        ]
      }
    ],
    "updateCommitment": "EiDZ8Q1s0oFanHWZlk8pJMgDsoDy5U_Vhxu30oAhE2UT7w"
  },
  "suffixData": {
    "anchorOrigin": "https://orb.domain1.com",
    "deltaHash": "EiA6iXqRuLckjaMn973OpMllBmQ76cZv5T1e2FP1eks_bw",
    "recoveryCommitment": "EiB4775MwCDFtbH1FSWiSu2nJcpeUnVzkU81o00u10FQQQ"
  },
  "type": "create"
}
```

## Identifiers

**Endpoint:** /sidetree/v1/identifiers/[id]

### GET

Resolve a DID document as per [Sidetree DID Resolution](../sidetree.html#did-resolution)

**Example**

Resolve a DID document using a canonical DID:

```
GET /sidetree/v1/identifiers/did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q HTTP/1.1
Host: orb.domain2.com
Accept: application/ld+json
Accept-Encoding: gzip, deflate
```

Response contains the DID document:

```json
{
  "@context": "https://w3id.org/did-resolution/v1",
  "didDocument": {
    "@context": [
      "https://www.w3.org/ns/did/v1",
      "https://w3id.org/security/suites/jws-2020/v1",
      "https://w3id.org/security/suites/ed25519-2018/v1"
    ],
    "assertionMethod": [
      "did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q#auth"
    ],
    "authentication": [
      "did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q#recoveryKey"
    ],
    "id": "did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q",
    "service": [
      {
        "id": "did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q#didcomm",
        "priority": 0,
        "recipientKeys": [
          "2qN15Qwk1uEi19BYJ19zshrxLHkKa5QvvXkLjdn56AaX"
        ],
        "routingKeys": [
          "3kRwP6VuwcbNSzevReUpfDWwyVGRS4YtqxB69DAs3VFN"
        ],
        "serviceEndpoint": "https://hub.example.com/.identity/did:example:0123456789abcdef/",
        "type": "did-communication"
      }
    ],
    "verificationMethod": [
      {
        "controller": "did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q",
        "id": "did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q#recoveryKey",
        "publicKeyJwk": {
          "crv": "P-256",
          "kty": "EC",
          "x": "MhxmsoUl3ACv0aZSJejEp6kxEO7QzSHedcYKN-3o0xw",
          "y": "7-TWvrUbnBkFFrqZ_nqVwFTgQyJE_mg11e3ckUEThvQ"
        },
        "type": "JsonWebKey2020"
      },
      {
        "controller": "did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q",
        "id": "did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q#auth",
        "publicKeyBase58": "4bkaQc1vU79nTbzUwyBSYtiv2pbb9mTYxHP1A7hGbyU3",
        "type": "Ed25519VerificationKey2018"
      }
    ]
  },
  "didDocumentMetadata": {
    "canonicalId": "did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q",
    "equivalentId": [
      "did:orb:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q",
      "did:orb:hl:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:uoQ-BeEJpcGZzOi8vYmFma3JlaWRxbWtkZHlyMmxrNmd4NWJ4enJneGdraHRqczNzcHJ5bmtkaHd3bGkzN2l4eXhzbHB5bnU:EiAzXL_RbZx3RCM4aThZ0-QeP0L9x7eoYwJBK2n_a0Um5Q"
    ],
    "method": {
      "anchorOrigin": "https://orb.domain1.com",
      "published": true,
      "publishedOperations": [
        {
          "anchorOrigin": "https://orb.domain1.com",
          "canonicalReference": "uEiDb2k2i5gYlgTg8GA8gN7apTp_JOkLgUCDY9A5eLOTXsQ",
          "equivalentReferences": [
            "hl:uEiDb2k2i5gYlgTg8GA8gN7apTp_JOkLgUCDY9A5eLOTXsQ:uoQ-BeEJpcGZzOi8vYmFma3JlaWczM2pnMmZ6cWdld2F0cXBheWI0cWRwbnZqajJwNHNvc2M0YmljYndodWJ6cGN6emd4d2U"
          ],
          "operationRequest": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJhZGQtcHVibGljLWtleXMiLCJwdWJsaWNLZXlzIjpbeyJpZCI6ImNyZWF0ZUtleSIsInB1YmxpY0tleUp3ayI6eyJjcnYiOiJQLTI1NiIsImt0eSI6IkVDIiwieCI6IjFwLU1sU0psTHowM05JemRuWWVTUnhMQ3dUNXhLQUpSQTVlcWd1TUIxOFEiLCJ5IjoiUFhXdXRsV3hRM3hDLTYwbExaRkc1REpZR2VNcVFUSDNUQ21jMDg1WFlHSSJ9LCJwdXJwb3NlcyI6WyJhdXRoZW50aWNhdGlvbiJdLCJ0eXBlIjoiSnNvbldlYktleTIwMjAifSx7ImlkIjoiYXV0aCIsInB1YmxpY0tleUp3ayI6eyJjcnYiOiJFZDI1NTE5Iiwia3R5IjoiT0tQIiwieCI6IkZfMFZfTG1Fel9qZVBFbFJmYXBEdzlvbHphbEo1SjJ2MW44MVpkVWVkWU0iLCJ5IjoiIn0sInB1cnBvc2VzIjpbImFzc2VydGlvbk1ldGhvZCJdLCJ0eXBlIjoiRWQyNTUxOVZlcmlmaWNhdGlvbktleTIwMTgifV19LHsiYWN0aW9uIjoiYWRkLXNlcnZpY2VzIiwic2VydmljZXMiOlt7ImlkIjoiZGlkY29tbSIsInByaW9yaXR5IjowLCJyZWNpcGllbnRLZXlzIjpbIkFrc3hOcnVrcUpBUkZQVENrUVFOZHprWjZpaFhLbXNQUHg3Z1M5cXBKZ3pDIl0sInJvdXRpbmdLZXlzIjpbIkRpRloxNG00M3FCa1VTVHNaWFR2U2oxQjVYQmR2UkptR2dINkZQVmM0bXdNIl0sInNlcnZpY2VFbmRwb2ludCI6Imh0dHBzOi8vaHViLmV4YW1wbGUuY29tLy5pZGVudGl0eS9kaWQ6ZXhhbXBsZTowMTIzNDU2Nzg5YWJjZGVmLyIsInR5cGUiOiJkaWQtY29tbXVuaWNhdGlvbiJ9XX1dLCJ1cGRhdGVDb21taXRtZW50IjoiRWlEWjhRMXMwb0ZhbkhXWmxrOHBKTWdEc29EeTVVX1ZoeHUzMG9BaEUyVVQ3dyJ9LCJzdWZmaXhEYXRhIjp7ImFuY2hvck9yaWdpbiI6Imh0dHBzOi8vb3JiLmRvbWFpbjEuY29tIiwiZGVsdGFIYXNoIjoiRWlBNmlYcVJ1TGNramFNbjk3M09wTWxsQm1RNzZjWnY1VDFlMkZQMWVrc19idyIsInJlY292ZXJ5Q29tbWl0bWVudCI6IkVpQjQ3NzVNd0NERnRiSDFGU1dpU3UybkpjcGVVblZ6a1U4MW8wMHUxMEZRUVEifSwidHlwZSI6ImNyZWF0ZSJ9",
          "protocolVersion": 0,
          "transactionNumber": 0,
          "transactionTime": 1645800397,
          "type": "create"
        },
        {
          "anchorOrigin": "https://orb.domain1.com",
          "canonicalReference": "uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ",
          "equivalentReferences": [
            "hl:uEiBwYoY8R0tXjX6G-YmuZR5pluT44aoZ7WWjf0XxeS34bQ:uoQ-BeEJpcGZzOi8vYmFma3JlaWRxbWtkZHlyMmxrNmd4NWJ4enJneGdraHRqczNzcHJ5bmtkaHd3bGkzN2l4eXhzbHB5bnU"
          ],
          "operationRequest": "eyJkZWx0YSI6eyJwYXRjaGVzIjpbeyJhY3Rpb24iOiJhZGQtcHVibGljLWtleXMiLCJwdWJsaWNLZXlzIjpbeyJpZCI6InJlY292ZXJ5S2V5IiwicHVibGljS2V5SndrIjp7ImNydiI6IlAtMjU2Iiwia3R5IjoiRUMiLCJ4IjoiTWh4bXNvVWwzQUN2MGFaU0plakVwNmt4RU83UXpTSGVkY1lLTi0zbzB4dyIsInkiOiI3LVRXdnJVYm5Ca0ZGcnFaX25xVndGVGdReUpFX21nMTFlM2NrVUVUaHZRIn0sInB1cnBvc2VzIjpbImF1dGhlbnRpY2F0aW9uIl0sInR5cGUiOiJKc29uV2ViS2V5MjAyMCJ9LHsiaWQiOiJhdXRoIiwicHVibGljS2V5SndrIjp7ImNydiI6IkVkMjU1MTkiLCJrdHkiOiJPS1AiLCJ4IjoiTlhvVWJrWFJnbWpJcnIzNzZxbTVxaDJKUFpXREhxRkVNV3dwdzE1LURFQSIsInkiOiIifSwicHVycG9zZXMiOlsiYXNzZXJ0aW9uTWV0aG9kIl0sInR5cGUiOiJFZDI1NTE5VmVyaWZpY2F0aW9uS2V5MjAxOCJ9XX0seyJhY3Rpb24iOiJhZGQtc2VydmljZXMiLCJzZXJ2aWNlcyI6W3siaWQiOiJkaWRjb21tIiwicHJpb3JpdHkiOjAsInJlY2lwaWVudEtleXMiOlsiMnFOMTVRd2sxdUVpMTlCWUoxOXpzaHJ4TEhrS2E1UXZ2WGtMamRuNTZBYVgiXSwicm91dGluZ0tleXMiOlsiM2tSd1A2VnV3Y2JOU3pldlJlVXBmRFd3eVZHUlM0WXRxeEI2OURBczNWRk4iXSwic2VydmljZUVuZHBvaW50IjoiaHR0cHM6Ly9odWIuZXhhbXBsZS5jb20vLmlkZW50aXR5L2RpZDpleGFtcGxlOjAxMjM0NTY3ODlhYmNkZWYvIiwidHlwZSI6ImRpZC1jb21tdW5pY2F0aW9uIn1dfV0sInVwZGF0ZUNvbW1pdG1lbnQiOiJFaUFsczR5V2xGRWIwWHBkYlViVjVpODY0TE9VX05WMGxFb3pyYmFoN2syMWxnIn0sImRpZFN1ZmZpeCI6IkVpQXpYTF9SYlp4M1JDTTRhVGhaMC1RZVAwTDl4N2VvWXdKQksybl9hMFVtNVEiLCJyZXZlYWxWYWx1ZSI6IkVpQzdPcDJTcUlJR1FEX2xRWEhIa0xKOWo3ODJfVTNGTVExeGNQVDAxNzQyQXciLCJzaWduZWREYXRhIjoiZXlKaGJHY2lPaUpGVXpJMU5pSjkuZXlKaGJtTm9iM0pHY205dElqb3hOalExT0RBd016azRMQ0poYm1Ob2IzSlBjbWxuYVc0aU9pSm9kSFJ3Y3pvdkwyOXlZaTVrYjIxaGFXNHhMbU52YlNJc0ltRnVZMmh2Y2xWdWRHbHNJam94TmpRMU9EQXdOams0TENKa1pXeDBZVWhoYzJnaU9pSkZhVVJrUkRWR1VtRjJNMDFhYnpGWGFHNU5WRmhVV201NVJIbGpXVXR3UjNSWFVWYzBSVU5zY2poUWVsUkJJaXdpY21WamIzWmxjbmxEYjIxdGFYUnRaVzUwSWpvaVJXbENUbXh5ZEhJeVJucFFkM1pCVUZFeGMyaERPR3hGVG1GT2EyMXpTR1Z3UXpOR1JHUjJiek5EVERoWlFTSXNJbkpsWTI5MlpYSjVTMlY1SWpwN0ltTnlkaUk2SWxBdE1qVTJJaXdpYTNSNUlqb2lSVU1pTENKNElqb2lSMk5LVW10TFREQk5aVnBLYnpWTVZHRndjVTl1YmxGamNFMWZWR3d4U0RGamVteFpWMDVUYkdoMU9DSXNJbmtpT2lJeWVpMXRhRlJyT0VGcFVISmZlWEJTUmpodkxVOTFkelpKZFRaUlEwUnZhSEZMYURGeGJsVlBhelF3SW4xOS5vcDVCR3Q5cHF6UEdJQml2R1RiYzBJMkdtSzN0dmpBRzE3NERac2VkWTRMT0NHdkk3RDI3MlVGZFNZMWRIVlYxdkI4dm5vNzdUWkNadDdyY3FMVno4USIsInR5cGUiOiJyZWNvdmVyIn0=",
          "protocolVersion": 0,
          "transactionNumber": 0,
          "transactionTime": 1645800399,
          "type": "recover"
        }
      ],
      "recoveryCommitment": "EiBNlrtr2FzPwvAPQ1shC8lENaNkmsHepC3FDdvo3CL8YA",
      "updateCommitment": "EiAls4yWlFEb0XpdbUbV5i864LOU_NV0lEozrbah7k21lg"
    }
  }
}
```
