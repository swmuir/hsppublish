Model Quick Reference

The following table provides a quick overview of all types supported by the Marketplace specification.

| Type                        | Brief Description                     |
| ----------------------------|:--------------------------------------|
|IdentityProvider|Marketplace instance-level configuration parameters allowing user authentication against an external authority using OpenID Connect authentication flows.
|User|A distinct person or system with some degree of access or interest to a Marketplace instance.
|(User) Identity|IdentityProvider-specific information pertaining to a given User.
|(User) Platform|Declaration of a compatible external Service runtime environment maintained by the User.
|Group|A named collection of Users for purposes of batch Role assignment.
|(Group) Member|Essentially a “join” record signifying a given User’s placement within a Group.
|Role|A named set of permissions.
|(Role) Appointment|The granting of a single Role to a single User or Group. (It is a polymorphic type.)
|JsonWebToken|An RFC 7519 JSON Web Token issued to permit access by a given Identity to a Marketplace instance as a bearer token.
|License|A known software or content license type, required to create Service records.
|Service|Declaration of a Platform-compatible executable in the form of key metadata. Does not directly provide a reference to an executable image.
|(Service) Screenshot|Optional graphical images for illustrating Service features to Users.
|(Service) Build|Defines the reference to a specific versioned image of a given Service. Images must be hosted such that the Marketplace and its Users have read-only network access, at minimum.
|(Service Build) Dependency|Known dependencies that are needed to run a given Build of a service. 
|(Service Build) Exposure|The standardized Interfaces capabilities provided by a given Service Build.
|(Service Build Exposure) Parameter |States that configuration parameter of the given name is required at runtime to successfully provide the Interface of the Exposure.
|(Service Build) Configuration|The runtime constraints of a Build that need to be known by a Platform for execution.
|(Service Build Configuration) Task|A container entry point and associated constraints that must be run as part of a Configuration profile.
|Interface|Marketplace-wide declaration of a standardized – or at least conventionalized – computational integration point. They are not constraints to HL7 standards.
|(Interface) Surrogate|Marketplace-wide statement that the referenced substitute Interface provides compatible capabilities of the given base Interface. Useful for defining new versions of an Interface that are backwards compatible with older versions.
