# TrustBlooc OpenID Connect for Verifiable Presentations

This is a WIP document of Draft [OpenID Connect for Verifiable Presentations](https://openid.net/specs/openid-connect-4-verifiable-presentations-1_0.html) 
implementation in TrustBloc platform.


## Sequence diagram
```mermaid
sequenceDiagram
    autonumber
    actor user as User
    participant verifier as Verifier
    participant vcs as Verifier VC-API
    participant wallet as Wallet Web
    verifier ->> user : Show `Share` button or `QR Code`
    user ->> verifier: Click on Save button or Scan QR code
    verifier ->> wallet : HTTP Redirect oidc /auth <br/>[Pass PEx definition in claims query param]
    wallet ->> wallet : search for VCs to match PEx
    wallet ->> user : show preview and ask for consent
    user ->> wallet : consent
    wallet ->> verifier : HTTP Redirect oidc /callback <br/>[Pass PEx Submission in vp_token query param]
    verifier ->> vcs : validate the presentation
    vcs ->> verifier : success
    verifier ->> user : show success page
```