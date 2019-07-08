# Digital Rights Management

## Product Rights
The ecosystem architecture will adopt several IP rights concepts friendly to both open access and Open Source-licensed products as well as commercial products. While Digital Rights Management (DRM) considerations are debatedly optional in the former cases, they are necessary for commercial ISVs and content vendors to have reasonable protection surrounding their work.

To provide concrete support for the most common payment models, several models related to product-level DRM are provided to form the basis of dynamic product authorization tracking and operations.

## Entitlements
Almost all consumer daily interactions with digital content are licensed. Many exceptions exist, such as for Government works and public domain materials outside of copyright, but the average person interacts primarily with copyrighted, licensed works. “Open Source” software under Apache 2.0, MIT, BSD license etc is still licensed for use, even though it is “free”. In order words, lack of financial transaction does not imply lack of binding legal obligations.

In the world of “closed source” digital content, license rights tend to be temporary. For example, renting a movie for online play may only allow for playback within 24 hours from start of play, 1 week from time of rental, 2 restarts, during a special promotional period, or other complex terms. Materials for trial or demonstration use may have similar restrictions, while access to software applications is enforced in more diverse ways. 

## Product Families
Products are often released in collections, hierarchical "families", or promotional groupings. The API allows arbitrary parent-child nesting of products to support automatic authorizations without needing to explicitly grant Entitlements to every layer in the product tree.

### Licenses

A "License" defines usage terms, regardless of the commercial nature of the product. Each Licenes definition is of one of the following types that governs the way Entitlements to it are enforced:

- Indefinite: No time-based restrictions. (e.g. F/OSS products, shareware)
- Relative: Only valid for a period relative to the time the Entitlement is created. (E.g. rentals, evaluation copies)
- Absolute: (E.g. end of calendar year only, a specific sale date only, etc)

A given Product may be offered under any number of Licenses. Operators may define business rules governing which types of Licenses are offered, and when.

When a user acquires a License to a Product, that record constitutes an "Entitlement". The entitlement is bound to the consumer acquiring the Product, and carries additional metadata.

All marketplace operators need to decide if using entitlement management applies to their use cases. For national-level resources, the recommendation is to support the most diverse set of licensing models as possible for both F/OSS and commercial works. The specification supports all of the following revenue models:



| <content id="table">Business Model Revenue Centers<content>        | Example           |
| ------------------ |:-----------------------|
|Service & Support (e.g., for publicly supported offerings) Support tickets, custom development|Red Hat, MITRE
|Subscription Recurring | MS Office, Netflix
| Freemium/Pro Advanced features, plugins, SLAs |GitHub, Dropbox, Skype
| Upfront 1-time charge per major version | Kindle books, Physical/Immutable goods, mobile apps
| Usage Per utility unit: e.g. CPU-hour, transaction, user, object etc | Amazon, Google, Azure, Oracle
| Advertising 3rd-party ad views, pay-per-click | Google search, “free” games

## Dynamic Authorization
Marketplaces implementing the Entitlement portion of the API carry several functions for both (1) local clients/platforms and (2) products themselves to check for valid entitlement. This authorization mechanic is referred to as "claiming" an Entitlement. Marketplaces may provide the baseline simple verification that the Entitlement is valid, or complex enhancements for directly handling financial transactions if needed.

Marketplaces that support diverse usage-based or metered business monetization models (as described in the [Business Model Revenue Table](DigitalRightsManagement.md#table) above) may use the entitlement and claim capabilities to implement technical solutions corresponding to the business models identified in  [Business Model Revenue Table](DigitalRightsManagement.md#table). 

For example, to: Inject raw object data when products are deployed. Create time-based constraints.
Meter for certain parameters periodically, such as local user count, local CPU count etc. Monitor for possible abuse, such as account sharing and/or excessive claims.

This simple mechanism aims to provide the core capabilities for F/OSS and commercial products to co-exist under the same set of operational principles.

The distinction is not as simple as “F/OSS vs commercial”, with exception cases and a broad grey area. For example, consider the less common licensing cases:
F/OSS works that require upstream reporting. This may require entitlement management even though the upstream vendor does not require an external financial transaction to have taken place.
Entitlements to commercial works that cannot be purchased, and must be granted by the vendor. This may occur when a vendor:
releases updates to a legacy product
offers a special product offered only via voucher
incentive such as marketing signup. (E.g. timeshare presentations)    

### Claims

An Entitlement represents the right to "claim" a Product's set of assets, according to the limitations of the specific ProductLicense that was purchased. A given Entitlement -- not the assets themselves -- are always owned by exactly one User. However, _Claims_ of the Entitlement are what is counted against the limits of the License. Additionally, Users may included in any number of Groups, when desirable. This approach should be used when Claims need to be counted at an aggregate level -- such as for organizational licensing -- and do not need to track User-level access.

Entitlements need not be immediately used. They may be purchased in advance, or by user account completely different from the consumer of the Product. When an end user is ready to use the Product, the Product and/or runtime environment may make an API call to marketplace API to verify user authorization using the claim object data injected via environment variables.

Each authorization call creates an Attempt record for operator reporting and vendor auditing purposes.

### Voucher
Vendors often distribute evaluation copies of products via digital codes. Each code is a human-readable public identifier for a precreated Voucher that is bound to exactly one ProductLicense. Thus, Vouchers are *not* a form of "gift card" that may be redeemed for unrelated Products, nor the _same_ Product under a different ProductLicense. This allows for several related use cases:

- Channel partner and resale by 3rd-parties. The end user may acquire the voucher through external means and comes back to the issuing marketplace instance for redemption.
- Offline sales. Internet access is not required for complex manual purchase order processes.
- Promotional giveaways. Voucher codes may be printed and shared via paper, email etc.





### External Transactions
Early feedback on the concept of an ecosystem revealed inconsistent perspectives for incorporating payment processing directly into this specification, as not all operators are capable of providing (let alone interested in) “checkout” processes often present in commercial platforms. For this reason, the approach of allowing, but not requiring, an ordering process has been adopted.

For products requiring an explicit ordering/checkout process for Entitlement, end users should be redirected to an external ordering URL provided by the ISV. When the user completes checkout, the ISV creates an Entitlement via marketplace API prior to redirecting them back to the marketplace. The allows for extremely flexible sales processes while preserving a marketplaces ability to manage Entitlements.

The ability to to externalize payment processing allows for operators to optionally avoid the complexities, administrative and legal headaches of being a middleman in a resale arrangement if they so choose. In those cases, Focus may then remain on curation and validation activities. Alternatively, operators may do the complete opposite by electing to fully manage the ordering process on behalf of ISVs. Both are acceptable operational models.


## Dependencies and Sublicensure
Interoperable, computable clinical content is enabled by common underlying terminology systems and information models. Presence of required dependencies unfortunately does not imply the consumer holds the right to use those dependencies.

This is further complicated by a jungle of terminology-level licensing models. NLM’s UMLS license defines a tiered class system of licensing terms based on the nature of a work, and is used by RxNorm, VSAC, and many others. US users may only acquire SNOMED CT through UMLS license. No means of organization-level licensing is provided, nor a means of redistributing UMLS content: not even for Government organizations. It is woefully inadequate and presents significant headaches for content producers wanting to publish computable content with “plug ‘n’ play” ease. Unfortunately, most informaticians seem to ignore the issue. Violations are commonplace.

The API does not attempt to model all underlying dependency licenses, as this is an implausable task. Rather, operators should establish clear submission policies to set expectations for what is/isn't permissable.

