tags:: Azure, Identity, Insight

- [[Azure Identity Protection]]
- [[Azure AD]]
-
- Identity becomes a critical piece for other security checklists
- Identity access schemas are hard to change once you create them
-
- [[Azure AD]] Integration scenarios
	- cloud identity
		- independent cloud identity, just AAD
	- Synchronized AD (more hybrid)
		- azure ad connect sync
		- syncs to on prem AD
		- single identity, enabling a same or single sign on experience through
			- [[Password Hash Sync]]
			- [[Pass-through Authentication]]
	- Federated Identity
		- single federated identity
-
- Hybrid authentications
	- [[Password Hash Sync]]
	- [[Pass-through Authentication]]
-
- Do's and Don'ts
	- DO understand all the options (and associated limitations or overheads)
	- DO always enable [[Password Hash Sync]], even if federated for backup
	- DO enable SSO with PHS and PTA
	- DO
	- DO
	- DON'T default to federated
- see slide 18ish
-
- Azure
- Azure AD connect sync
	- see supported topologies, slide 20
-
- look up azure AD connect sync and cloud connect sync differences. Slide 24
	- larger directories are better for AD connect sync
	-
- A forest is a combination of multiple Active Directory domains
-
-