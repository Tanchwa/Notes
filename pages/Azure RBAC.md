- compose of 3 things
	- service principle (service account)
	- assign a role
	- and a scope
		- a set of resources
- best practices
	- segregate duties to assign granular permissions, one control for every type of permission
		- if I am allowed to drain a database, I shouldn't be able to delete the logs that I did it
	- use least privilege
- Common Roles
	- Owner
	- Contributer
	- Reader
	- Group
	- Service Principal
- Azure has so many RBAC roles already in existence that you can just use pre-made ones
	- azadvertizer.net #AZLinks