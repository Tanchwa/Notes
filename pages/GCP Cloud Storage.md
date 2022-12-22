- object storage, similar to [[Azure Blob Storage]] or [[AWS S3]]
- use for
	- content delivery
	- data lakes
	- backup
- separated into [[GCP Storage Class]]es
-
- id:: 639a380c-0d5d-44f3-b0cb-4d1fee3af237
  #+BEGIN_TIP
  Large files can be transferred to GCP even over low bandwidth WAN connections with Parallel composite uploads https://cloud.google.com/storage/docs/parallel-composite-uploads
  #+END_TIP
-
- Availability Choices
	- Region
	- Dual Region uses pairs of regions
	- Multi Region [[GCP Multi Region]]
-
- Object Versioning and Lifecycle Management
	- objects are immutable
	- object versioning can be enabled, and will allow you to create a new version of the object and keep the old
	- versions can add up, so we use Lifecycle Management
	- can auto delete items based on configurations such as
		- setting TTL
		- delete or archive non-current versions
		- downgrade storage class
	- and then take action on it based on rules
	- ![Screen Shot 2022-12-12 at 7.25.11 PM.png](../assets/Screen_Shot_2022-12-12_at_7.25.11_PM_1670891133296_0.png)
	- lifecycle rules can take up to 24 hours to take effect
- ![](https://download.logo.wine/logo/Google_Storage/Google_Storage-Logo.wine.png){:height 597, :width 883}