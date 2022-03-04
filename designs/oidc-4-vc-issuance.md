# TrustBlooc OpenID Connect for Verifiable Credential Issuance 

This is a WIP document of Draft [OpenID Connect for Verifiable Credential Issuance](https://openid.net/specs/openid-connect-4-verifiable-credential-issuance-1_0.html) 
implementation in TrustBloc platform.


## Sequence diagram

```mermaid
sequenceDiagram
    autonumber
    actor user as User
    participant issuer as Issuer
    participant vcs as "Issuer VC-API"
    participant wallet_web as "Wallet Web" 
    participant wallet_server as "Wallet Server" 
    issuer ->> user : show `QR Code` or `save button`
    user ->> issuer: Scan `QR Code` or click on `save button`
    issuer ->> wallet_server: HTTP Redirect /initiate_issuance
    wallet_server ->> issuer : HTTP GET credential_manifest (along with PEx def) by discovery endpoint
    issuer ->> wallet_server: HTTP Response credential manifest
    wallet_server ->> issuer : HTTP Redirect oidc /authorize (pass PEx submission)
    issuer ->> user : login prompt
    user ->> issuer : login details
    issuer ->> issuer : validate PEx submission
    issuer ->> wallet_server : HTTP Redirect oidc /callback
    wallet_server ->> issuer : HTTP Post /token endpoint
    issuer ->> wallet_server : HTTP Response access token
    alt Defered VC flow
    wallet_server ->> issuer : HTTP POST /credential endpoint
    issuer ->> wallet_server : HTTP Response with acceptance_token
    wallet_server ->> wallet_web : pass acceptance_token
    wallet_web ->> issuer : HTTP POST /credential_deferred with acceptance_token
    issuer ->> vcs : HTTP /issue-credential API
    vcs ->> issuer : HTTP response signed VC
    issuer ->> wallet_web : HTTP Response Verifiable Credential
    else Direct VC flow
    wallet_server ->> issuer : HTTP POST /credential endpoint
    issuer ->> vcs : HTTP /issue-credential API
    vcs ->> issuer : HTTP response signed VC
    issuer ->> wallet_server : HTTP Response Verifiable Credential
    end
    wallet_server ->> user : Show VC and ask for consent to save
    user ->> wallet_server : Consent
    wallet_server ->> user : show success page
```