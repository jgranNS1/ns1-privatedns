## 2.1.0 (Aug, 19 2019)
- What's New?
   - Renamed `web` to `core` container
   - Renamed `cache` to `dist` (distribution) container to disambiguate its function
   - Increased password complexity requirements
   - Enhanced security for API key secrets
   - Added new database layer for increased capacity and protections for referential integrity
   - Extended portal navigation to include IPAM and DHCP pages
   - Added ability to create and manage IP address objects including split, merge, bulk delete
   - Added ability to search and filter subnets
   - Added IPAM user-defined metadata in the form of tags and custom attributes
   - Added ability to create and modify DHCP server and scope settings and options
   - Added view and manage permissions for IPAM and DHCP
   - Improved performance of container configuration daemon to reconfigure in seconds
   - Added support for customized data replication across networks
- Known Issues
   - A zone in two or more networks cannot be re-pooled; the workaround is to delete the zone and recreate it with new network pool value(s); zones in a single network pool can be re-pooled without this workaround
   - A /32 or /128 host currently assigned with a reservation cannot be deleted from the IPAM interface; the workaround is to delete the corresponding reservation first before deleting the address objects
   - Container configuration web interface: Configuration lists do not allow reconfiguration; instead of using the web interface to modify these lists, the workaroud is to use the CLI or HTTP REST API configuration (e.g. docker exec -it dns supd run --data_service_defs 1-10); this is applicable to lists of data_peers in the data container and data_service_defs in the dns and dist containers.
   - Multiple scope groups should not be assignable to a single DHCP service. If this action is taken, the DHCP service (Service Definition) will be returned multiple times in the dropdown for scope & reservation assignment and or when editing settings on the DHCP service.
   - Creating a zone w/o a service group or corresponding DNS pool existing returns a 500 error; workaround the limitation by creating a service group, associating it with the organization, and defining a DNS pool.
   
## 1.1.1 (Apr 18, 2019)
- What's New?
   - Added ability to configure zone and record pagination limits (i.e. beyond 2500) of `web` containers
   - Miscellaneous UI and UX improvements to configuration pages
- What's Fixed?
   - Fixed issue with `web` container health checks resulting in false positives
   - Fixed issue where operator users logged into the portal could not create users, apikeys, or teams on behalf of an organization
   - API and In-Memory database no longer need to be restarted after a `data` container failover event
   - Miscellaneous UI bugs in the NS1 portal

## 1.1.0 (Nov 2, 2018)
- What's New?
   - Added failover support for data container; operate a "primary" and one or more "replica" data containers to achieve this configuration
   - Added `data cache` container to act as a local copy of `data` at the edge for very distributed deployments
   - Added support for Promethius as a target for exporting operational metrics
   - Added TLS options for exporting operational metrics to other systems
   - Added Cost filter to supported filters in Traffic Management category
   - Added action to `Restore Main Database` in the `data` container's configuration page
   - Added json support to supd commit endpoint allowing scripting of configuration changes across many containers at once
   - Added ability to view real-time stream of container logs in the supd web UI
   - Added more actions to restart individual container services
   - Miscellaneous UI and UX improvements to configuration pages
- What's Fixed?
   - Fixed issue with data container hostname; see 1.0.3 Known Issues
   - Fixed issue with recursor caching of zones for which the system is authoritative
   - Fixed issue with GeoIP file upload missing the option for ASN `.mmdb` files
   - Miscellaneous UI bugs in the NS1 portal
- Known Issues
   - `dns` containers in `Recursive Resolver` mode does not support ECS client subnet; this means for zones which the system is authoritative certain filters (Geotarget, Geofence, and Netence filters) ignore client IP

## 1.0.4 (Sept 21,2018)
- What's Fixed?
   - Allow creation of RFC 1918 reverse DNS zones for private IPs

## 1.0.3 (Aug 24, 2018)
- What's Fixed?
   - Config options for forwards now distinguishes between forwards to recursive resolvers and forwards to authoritative servers
   - Fixed action: "Restart In-Memory Database"
   - Fixed inability to delete forwards
   - Fixed inability to configure forwards via CLI
- Known Issues
   - Once a data container hostname is set it cannot be changed internally or the data will be unreadable; we recommend operators keep the hostname the same after first standing up the data container

## 1.0.2 (Aug 8, 2018)
- What's Fixed?
   - Bugs squished with filter chain configuration
   - Password change endpoints and 2fa re-enabled for portal users

## 1.0.1 (Aug 1, 2018)
- What's Fixed?
   - Actions no longer clear changes made to configuration manager
   - Added per process operational metrics
   - Miscellaneous UI bugs in the NS1 portal
- Known Issues
   - Password changes must be performed via API call; these functions are unavailable in the portal interface at this time; see section 11.6 Reset a User’s Password

## 1.0.0 (Jul 25, 2018)

- What's New?
   - First generally available version of Private DNS
   - Added certificate management for unified transport layer security
   - Added Basic Authentication credentials to access container configuration, initialized to ns1/private
   - Added operators, organizations, teams, users, and API keys
   - Added bootstrap, operator, and organization endpoints for multi-tenant support
   - Added recursive resolver mode and support for zone forwards to dns container
   - Added cache containers for scalability, resiliency, and performance improvement at the edge of distributed networks
   - Added axfr support for secondary zones
