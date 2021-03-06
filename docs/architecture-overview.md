# Software Design Verification Portal
by Alexander Stiefel (alexander.stiefel@t-systems.com)

##	Introduction
This document describes the component Verification Portal for the System “Corona Warn App”. In the world of the Corona Warn App the Verification Portal allows hotline employees to generate teleTANs which are used by users of the mobile App to upload their diagnostic keys. A teleTAN is a proof that somebody was tested positive for SARS-CoV-2.

This document links the overall system architecture with the software design of the Verification Portal, it links user stories with implementation inside the Verification Portal. 

This document is intended to be read by people who want to get insights how verification for hotline and health authorities works in detail, it is our guideline for implementation. This document is a mixture of software design and requirement specification, it may be splitted in future.

Please be aware that several aspects in this document are not final, certain important information is missing.

#	Overview
##	Purpose of the Software Component
The primary scope of the component is to create a proof certificate (teleTAN) whether users were positive tested for SARS-CoV-2 or not. The proof is generated by hotline employees based on the information the can obtain. The scope of the Verification Portal is just the creation of the proof certificate - the teleTAN. Not is scope is the support of validating the request of people calling the hotline.  


##	Context
The Verification Portal is part of the Portal Server. The Portal Server consists of this component and the Verification IAM and is a logical grouping of services and not a "Server" in the traditional sense. 

The context is:
- Verification Portal 
    - a web portal allowing to generate teleTAN
- Verification IAM
    - a Idenitity and Access Management software, managing users, right, roles and implementing two factor authentication
- One Time Password Generator
    - used to provide the 2nd factor for authentication
- Verification Server
    - System which trusts the Verification Portal and ultimatly generates the teleTAN


##	Core Entities
|Entitiy|	Definition|	
| ------------- |:-------------:|
|teleTAN|	Is a subtype of TAN with reduced length and life time. This TAN is handed over via phone and contains only uppercase letters and numbers, excluding 0,O and I,1. Length of teleTAN is 7 characters. The lifetime of a teleTAN is 1h. This entity is managed by Verification Server	|
|SARS-CoV-2 Test|	A SARS-CoV-2 Test aggregates several probes donated by a single person to verified whether they proof an infection with the SARS-CoV-2 virus or not. A SARS-CoV-2 Test can have multiple states, “positive”, “negative”, “unknow”, “erroneous”	|


# Software Design

## Technology Stack
1. Spring Boot
1. Docker images for deployment

## Security
Security of the portal is implemented by
1. tight integration with Verification IAM
2. using two factor authentication
1. manual process for onboarding new users

### Authentication
Authentication is based on the [OpenID specifications](https://openid.net/developers/specs/), which works on top of the OAuth 2.0 (RFC 6749) framework. OpenID uses Json Web Tokens (JWT) which are signed by the IAM to provide integrity and authenticity.

# Security Requirements

1. Session Timeout
The session timout is 4 hours.

1. Display timeout for teleTAN
A teleTAN must not be shown after 10min. 

1. All server to server communications must be authenticated with client certificates

1. Implement network connectivity between Verification Portal and Verification Server as special OTC route

# Implemented Use Cases
## Referenced User Stories
will be defailed in later versions of this document

##	Actors
1. Hotline Employee
Has technical role "c19hotline". Is an employee which mainly triggers the use case Create teleTAN. 
2. Employee Health Authority
Has technical role "c19healthauthority". Not implemented in the current release.
3. Verifion Portal User Admin 
Has technical role "verificationPortalAdmin". Administrator who executes user administation in the Verification IAM
4. Hotline Trustee
Has technical role "c19hotlineTrustee", but is not implemented in this component, is implemented in a manual eMail based flow. This actor approves the requests for user creation in the Verification IAM


## Use Cases

List of use cases
1. Use Case Create teleTAN
1. Use Case Login
1. Use Case Logout
1. Use Case Change Password
1. Use Case Change 2nd Factor Device
1. Use Case Password forgotten

# Data Model
This component has no data to persist.
