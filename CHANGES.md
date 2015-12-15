<!-- Note: keep this file in Github markdown format for easy pasting to the geni-tools wiki -->

# Release notes for gcf (geni-tools) [2.11](https://github.com/GENI-NSF/geni-tools/issues?q=milestone%3A2.11)

# Release notes for gcf (geni-tools) [2.10](https://github.com/GENI-NSF/geni-tools/issues?q=milestone%3A2.10)
 * Changed references to trac.gpolab.bbn.com to point to Github.
  * Although those pages mostly still reference trac, that is the future home. These changes include changing where the `agg_nick_cache` lives. (#814)
 * Add some omni design notes (work in progress). (#883)
 * Document how to create a release. (#871)
 * Add Debian and RPM packaging configuration (#849)
 * Update CentOS installation instructions (#853)

 * Omni
  * Continue anyway if no aggregate nickname cache can be loaded. (#822)
    * Sliver info reporting and operations on AMs by nickname will likely fail.
  * For connections to servers, use TLSv1 if possible (falling back to SSLv3
    in the usual OpenSSL way), plus a restricted set of ciphers
    (`HIGH:MEDIUM:!ADH:!SSLv2:!MD5:!RC4:@STRENGTH`) when using python2.7.
    This avoids some security issues, and allows Omni on older clients
    to connect to some updated servers. (#745)
  * Use `False` instead of `'f'` for `SLICE_EXPIRED` and `PROJECT_EXPIRED` when 
    using Common Federation API clearinghouses. (#856)
    * Thanks to Umar Toseef for the bug report.
  * Do not assume `PROJECT_MEMBER_UID` is returned when listing project members,
    but allow it. (#857)
    * Thanks to Umar Toseef for the bug report.
  * Calling `getslicecred` while specifying a `slicecredfile` that exists
    no longer means just return that file. Instead, that file will be
    ignored and, if you specify `-o`, replaced. (#868, #869)
  * Moved canonical `agg_nick_cache` location to Github. (#814, #882)
  * Use `urllib2.urlopen` instead of `urllib.urlretrieve` to avoid bad
    interaction with M2Crypto. (#881)

 * Stitcher
  * Catch expiration too great errors from PG AMs and quit. (#828)
  * Bug fix combining manifests involving a fixed endpoint. (#833)
  * Fix up combining manifests using ExoGENI, to ensure the stitching
    hops list the actual reserved VLAN tags. (#873, #876)
  * Support detecting OESS AM using `geni_am_type`. (#835)
  * Do not insist on 2 reciprocal properties on links, allowing more
    flexibility perhaps. (#838)
    * Allow links with more than 1 interface per AM, and start supporting
      multiple AMs per link.
  * Add option `--noSCS` to force skipping the SCS, for use when stitcher
    calls the SCS but the SCS fails the request, and the request is
    complete already. Or use this on the expanded request from a stitcher
    call with `--noReservation`. Lack of a workflow may be a problem
    in some cases. (#839)
  * AL2S supports speaks for. Don't exit if using speaksfor and AL2S. (#834)
  * Treat new generic ProtoGENI mapper error code (28) as fatal. (#861)
  * Fix combining manifests where link is multi AM but not stitched. (#879)
   * Thanks to Hussam Nasir

 * omni-configure
  * Modify omni-configure to support multi-user systems (#843, #877)

 * gcf
  * Add new parameters to decode_urns so that derived delegates can 
    decode urns using the credentials and options available in other 
    calls (#858)
  * Add utility function to derive the effective user's client cert, after
    accounting for speaks-for. This allows an AM to know the "real" user's URN
    and email, for example (#831)
  * Speaks for attempts to validate the tool cert by taking the issuer from the user
    GID (signer of the speaks for credential), in cases where the tool cert didn't
    validate on its own and has the same issuer as the user cert. This helps where
    tool certs and user certs are issued by the same intermediate CA (as with those
    from the GENI Clearinghosue), when used on webservers that don't pass the full
    client certificate chain (like nginx). (#832)
  * For connections to servers, use TLSv1 if possible (falling back to SSLv3
    in the usual OpenSSL way), plus a restricted set of ciphers
    (`HIGH:MEDIUM:!ADH:!SSLv2:!MD5:!RC4:@STRENGTH`) when using python2.7.
    This avoids some security issues, and allows older clients
    to connect to some updated servers. (#745)
  * Rework the code in `am3.py` that calls the cred verifier to reduce code duplication and make 
    implementations deriving from the ReferenceAggregateManager easier to write. (#836)
  * Allow aggregate deployments to specify the delegate to use in the
    `gcf_config` file or on the command line, so as to be able to
    write delegates without touching the main gcf code, thereby easing maintenance (#837).
  * When delegates raise an `ApiErrorException`, catch it to send back
    a proper return value indicating an error occured. (#841)
  * Log the HTTP request line when `--debug`. (#842)
  * Fix some typos in `Provision()` in `am3.py`. (#846)
  * Add Debian and RPM packaging configuration (#849)
  * Fix am2 not keeping resources' state updated when creating or deleting Slivers (#848, #850)
  * Update generated certificate key size and signature algorithm to more
    modern values (#851, #852)
  * Update CentOS installation instructions (#853)
  * Point people to gcf-developers@googlegroups.com instead of old list.
  * In AM3, fix exception on expire_slivers. Aggregate stores resources, not slivers. (#863)

# Release notes for gcf (geni-tools) [2.9](https://github.com/GENI-NSF/geni-tools/issues?q=milestone%3A2.9)
Released May 27, 2015
 * Add Markdown style README, CONTRIBUTING and CONTRIBUTORS files. (#551)
  * Source code is now available from Github: https://github.com/GENI-NSF/geni-tools.
    All issues are now reported on geni-tools as well. (#814)
    Issues after #820 are _only_ on Github.
 * Remove references to the omni-users mailing list. (#813)
  * Use the [GENI Users mailing list](https://groups.google.com/forum/#forum/geni-users).
 * Add autoconf/automake support
 * Add `utah-stitch` AM and `clemson-clab` and `wisconsin-clab` for new Cloudlab sites. (#817)

 * Omni
  * If `sliverstatus` fails in a way that indicates there are no local resources,
    and the caller specified `--raise-error-on-v2-amapi-error`, still delete any
    sliver info records at the clearinghouse (as necessary). (#778)
  * If `deletesliver` or `delete` fail indicating there are no local resources,
    delete any sliver info records at the clearinghouse (as necessary). (#782)
  * Add 11 aggregates to the nickname cache. (#783)
  * Trim fractional seconds from user input datetimes before passing to servers. (#795)
    * Avoids errors in ProtoGENI based code.
  * Allow getting Ad RSpecs (calling `ListResources` not in slice context)
    without a user credential. (#793)
    * Implements AM API Change Proposal AD.
  * If the return from `POA` is a `geni_credential`, or per sliver has a
    `geni_credential`, then save that cred in a separate file. (#803)
    * Also when saving output, JSON that has XML content shouldn't get
      the header inserted within the XML content.
  * Fix `delete` when called on multiple AMs. (#808)

 * Stitcher
  * Better support for AM API version 3 (#261)
    * When saving the slice credential renames the file (`.json` extension), save that.
    * Set the API version for an AM to the command line request version, or the most recent
      lower version number.
    * When picking the requested expiration time for an AM, limit it to 3 hours when doing Allocate.
    * Set the AM URL to the v3 URL if using v3
    * Tune the pause before redoing a request to be shorter if only dealing with v3 AMs
    * Warn when done if any reservations are only allocations.
    * Always use APIv2 when calling `getversion`
  * Support partial requests with two new options:
    * `--noDeleteAtEnd`: When specified, do not delete any successful reservations when the overall
      request has failed, or when the user has interrupted stitcher with Ctrl-C.
    * `--noTransitAMs`: When specified, stop when the only aggregates ready to reserve are those
      added by the SCS (which we assume are transit or intermediate aggregates).
    * In both these cases, finish by printing out how many reservations you have, and saving a
      combined manifest RSpec for your reservations, and a combined request RSpec for the reservations
      that you still need to make. The experimenter must manually edit this request to fill in the proper
      `suggestedVLANRange` and `vlanRangeAvailability` in the proper hops in the request's stitching extension.
      See the comments at the top of the RSpec: `get_vlantag_from` indicates what other `hop` the given `hop`
      should take its VLAN tag from. `Have Reservation?` indicates if you have a reservation here. And
      `AM Depends on` indicates which other AMs must be reserved first before you make a reservation here.
    * Clean up the log messages in these cases to not look so scary. (#812)
  * Support `delete` and `deletesliver` locally, calling APIv2 or v3 as appropriate. (#807)
    * Defer to Omni if there is only a single aggregate.
    * Each AM is a separate Omni call.
    * Results from all calls are combined into a single return message and structure.
    * APIv2 results will be keyed by AM URL and have the value of True or False usually, but sometimes a struct.
    * This is useful when a circuit was reserved using `-V3` and some AMs spoke APIv2 only.
    * Fix handling of return when tried to delete at an AM with no resources. (#823)
  * scs.py now honors `--timeout` to set the SSL connection timeout. (#785)
  * For `--fixedEndpoint` calls, avoid adding the fake interface to links
    with no stitching path, such as for a LAN within a single AM. (#790)
  * If SCS gave same suggested VLAN tag to 2 paths (same hop), then retry
    at the SCS. (#802)
    * The SCS does not deconflict suggestions across paths, so this can happen.
  * When a request fails due to VLAN unavailable someplace along a path where the VLAN
    was a choice from 'any', no need to go back to the SCS; delete the intermediate reservations
    and mark the failed tag unavailable, and let that first AM pick the tag again.
    This should mean fewer un-necessary deletes and fewer calls to the SCS. (#648)
    * Also fix this block to count this as a local VLAN retry.
  * When merging manifests, handle a request for component_manager of the ExoSM,
    where the manifest component_manager will be more specific. (#780)
    * If there isn't an exact match and the specific component_manager doesn't belong 
      to an aggregate we are using, then try to match without the sub-authority.
  * When redoing a request from the SCS, be sure to copy any existing state
    from an AM not in the SCS workflow. (#781)
  * When checking if an EG URN belongs to this AM, compare them exactly,
    not using `startswith`, so an EG rack doesn't claim a URN for the ExoSM. (#779)
  * In debug when printing raw return from SCS, print the name of the path for each hop. (#800)
  * Clarify error message when request 'any' VLAN and get VLAN_UNAVAILABLE. (#801)
  * Fix finding a hop when parsing EG manifests to consider the path ID, thus
    supporting 2 links across the same interface. (#805)
  * Fixes for merging manifests, particularly those including ExoGENI AMs on links. (#806)
    * Fill in missing properties and interface_refs on links when the template was a real manifest.
    * Slightly prefer PG AMs as the template AM.
    * Ensure we have all component_managers, interfaces, and comments on links.
    * Skip adding a comment for an AM with no information to add.
    * Support starting with an EG AM and adding in missing hops in the stitching extension, albeit
      out of order and without the proper nextHop values.
    * Bug fix for merging comments when template has a comment child and other does not. (#815)
  * Quiet down errors deleting a failed reservation from EG AMs (harmless). (#811)
  * Remove reference to ION as a real aggregate in the README. ION has been decommissioned. (#797)
  * Gracefully handle error when 2 stitcher instances are running in the same directory
    at once on Windows. (#824)
  * Define default initial sliver expirations for other Utah/PG AMs (Apt, Cloudlab, stitch). (#826)
    * Allow defining new such AMs in the omni config `omni_defaults` -> `utah_am_urns` (CSV).
  * When calculating `expires` time to request, don't exceed the slice expiration,
    and don't request less than some # of hours - 6 for APIv2, 3 for v3+. (#827)

 * Scripts
  * Initial commit of `examples/renewSliceAndSlivers.py`. (#798)
  * `readyToLogin --ansible-inventory` will now specify the ansible
    username in the inventory file if the GENI username and the
    username on the local system are different.  Alternatively, you
    can use `--ansible-username` to specify the username or you can use
    `--no-ansible-username` to ensure no username is specified in the
    inventory file. (#794)
  * Changed usage message for `readyToLogin` to encourage use of `--useSliceAggregates`. (#816)
  * Fix error message on `readyToLogin` when no aggregates specified. (#821)

 * gcf
  * Make the user credential optional to `ListResources`. (#792)
    * Implement AM API Draft proposal "AD".
    * `CredentialVerify` now requires a valid credential over the given `target_urn`
      if supplied, but returns `True` if no credentials are supplied AND no `target_urn`.
  * Print AM version when logging that GENI AM is listening. (#804)
    * Thanks to David Margery

# Release notes for gcf (geni-tools) [2.8.1](https://github.com/GENI-NSF/geni-tools/issues?q=milestone%3A2.8.1)
Released May 1, 2015
 * Give `omni-configure` the ability to find PKCS#8 private keys,
   not just PKCS#1. With openssl v1.0.0 PKCS#8 is the default, so new
   keys from the GENI Portal are in the new format, and omni-configure 
   doesn't find them and complains that you have no private key. (#799) 

# Release notes for gcf (geni-tools) [2.8](https://github.com/GENI-NSF/geni-tools/issues?q=milestone%3A2.8)
Released February 9, 2015
 * Omni
  * Allow configuring how many times Omni retries on a busy error from
    an AM or CH. Use `--maxBusyRetries`. Default remains 4. (#749)
  * Support `Update()` and `Cancel()` from AM APIv4 in any v3+ implementation. Support
    is only known at ProtoGENI, and is limited. (#589)
  * New option `--noCacheFiles` completely disables reading or writing the GetVersion and
    Aggregate nickname cache files. (#772)
  * New config that sets the current release number and a release message,
    so Omni can alert you if a new release is available. (#698)
  * Better control of Omni logging configuration. (#458)
    * Allow a Python logging configuration dictionary, and configure
      logging from that if possible.
    * New option `--noLoggingConfiguration` completely disables
      configuring Python loggers from Omni. A script might use this to
      allow it to configure logging later in its own way.
  * Fix error message on expired user cert. (#756)
  * Remove ticket #722 workaround (bug fixed at ION AM). (#724)
  * Mac installer: remove old aliases before adding new ones. (#556)
  * Clean up `listresources` summary string and include sliver expiration if known. (#704)
  * Add support for `--start-time` option to specify a `geni_start_time` option
    for any aggregates that support such a value. (#660)
  * Update copyrights to 2015 (#764)
  * Add nicknames for CloudLab and Apt. (#767)
  * Avoid exception on empty aggregate in `-a` argument. (#771)
  * Support python 2.7.9+ where we must request not verifying server certificates
    for the SSL connection. Thanks to Ezra Kissel. (#776)

 * Stitcher
  * Catch keyboard interrupts to warn if you interrupt your run and have 
    left over reservations. (#754)
  * Added `--timeout` option to specify that stitcher runs time out 
    (default is no timeout as before), deleting any partial reservations. (#730)
  * SCS requires a valid client certificate and uses SSL. (#757)
    * As a result, the SSL timeout on calls to the SCS should work. (#346)
    * When calling `scs.py` directly, you must now provide the `--key` and `--cert`
      arguments to reach the default SCS or any SCS that uses https.
  * Check current VLAN tag availability at AMs before trying reservations, where it
    works and will help. Disable this with new option `--noAvailCheck`. (#566,#545)
  * Better support for requests using multiple ExoGENI sites. (#738)
    * Use ExoGENI stitching between ExoGENI sites by default (as before).
    * Allow reserving resources from multiple individual ExoGENI sites,
      not through the ExoSM. This is a change.
    * Allow using GENI stitching between ExoGENI sites by going through individual
      ExoGENI sites and not the ExoSM. This is a change.
      * A request with no link among ExoGENI sites will not use the ExoSM, unless
        you specify `--useExoSM`. This is a change - previously, you would get the ExoSM.
      * A request that does include a link among ExoGENI sites will by default continue to
        use the ExoSM and ExoGENI stitching. But you can choose to use GENI stitching: 
        supply `--noExoSM` or `--noEGStitching` or `--noEGStitchingOnLink`.
      * Note however that passing options `--noExoSM` or `--noEGStitching` means that any links between ExoGENI
        sites will attempt to use a GENI stitched link, which may not be possible. Using
        `--noEGStitchingOnLink` means that only the named links between ExoGENI sites will attempt
        to use a GENI stitched link.
      * See README-stitching.txt for more on `--noEGStitching` and `--noEGStitchingOnLink`.
  * Allow recreating the combined manifest RSpec. (#284)
    * Call `stitcher listresources <slice>` or `stitcher describe <slice>`.
    * If you specify `--useSliceAggregates`, any local stitcher file listing AMs in the slice
      will be ignored.
    * Return matches that for Omni, with the combined manifest returned under the key
      `combined` for `describe`, and `('combined','combined')` for `listresources`.
  * Add new production AL2S server to nickname cache and support identifying the OESS AM
    using the new URL. (#775)
  * Use the SCS instance maintained by Internet2 by default. (#761)
  * Limit available range on a hop that doesn't import to tags available at hops that 
    import from it. (#747)
  * Support only generating and saving the expanded request. (#763)
    * Supply new option `--genRequest`.
    * No reservation is done, and no slice credential check is done.
    * Includes SCS expansion and checking current availability.
  * Handle PG AMs taking longer to delete a previous reservation. (#769)
    * Better handle delete errors that really mean there was nothing here to delete.
    * Before allocating, if stitcher did a previous delete at this aggregate in this run,
      check `sliverstatus` to ensure that delete completed.
  * `Invalid slice urn` is fatal at ExoGENI. (#746)
  * Remove note about capacity at ExoGENI, now that it is in kbps. (#678)
  * Error and exit if the request uses Speaks For and requires AL2S 
    (since AL2S does not support Speaks For yet). (#753)
  * Accept `--scsURL` in `scs.py` to specify the SCS URL. (#755)
  * Add logic to print status when deleting reservations. (#750)
  * Factor out determining if AM type supports requesting 'any' VLAN tag. (#758)
  * Allow `--verbosessl` to `scs.py` to turn on debug SSL logging. (#770)
  * Allow re-setting default sliver expiration times by aggregate type with a new
    `omni_defaults` value (like from the agg_nick_cache). (#694)

 * readyToLogin
  * Add `--ansible-inventory` option which creates a host file to use with Ansible. Works 
    with `-o` and `--prefix`. (#759)

 * remote-execute
  * Add `-A`/`--forwardAgent` option to enable SSH agent forward (like using `-A` with SSH). (#774)

 * gcf
  * Add support for policy-based authorizer to interpose between authentication
    and delegate invocation. (#660)
   * Allow ABAC-like per aggregate authorization policies. See README-authorization.txt
   * Support scheduled reservations. See README-scheduling.txt
   * Refactored the AM base class to use a Method Context to handle most per method
     processing and error handling.
     * Instead of wrapping calls to the delegate in a try/except block, we use a
       `with AMMethodContext` call to check and authorize arguments before making the call
       to the delegate.
   * Add a getter for fetching the PEM certificate from the server, to support safely fetching
     that certificate in a multithreaded environment.

# Release notes for gcf (geni-tools) [2.7](https://github.com/GENI-NSF/geni-tools/issues?q=milestone%3A2.7)
Released October 14, 2014
 * Omni
  * Calls to `status` and `sliverstatus` will also call the CH
    to try to sync up the CH records of slivers with truth
    as reported by the AM. (#634)
  * Make `useSliceMembers` True by default and deprecate the option. (#667)
    * By default, Omni will create accounts and install SSH keys for members of your slice, 
      if you are using a CHAPI style framework / Clearinghouse 
      which allows defining slice members (as the GENI Clearinghouse does). 
    * This is not a change for Omni users who configured Omni using the `omni-configure` script,
      which already forced that setting to true.
    * The old `--useSliceMembers` option is deprecated and will be
      removed in a future release.
    * Added new option `--noSliceMembers` to over-ride that default and tell Omni
      to ignore any slice members defined at the Clearinghouse.
    * You may also set `useslicemembers=False` in the `omni` section of
      your `omni_config` to over-ride the `useSliceMembers` default of True.
  * Honor the `useslicemembers` and `ignoreconfigusers` options in the `omni_config` (#671)
  * Fix `get_member_email` e.g. from `listprojectmembers` for speaks-for. (#676)
  * Fix nickname cache updating when temp and home directories are on 
    different disks - use `shutil.move`. (#646)
  * Look for fallback `agg_nick_cache` in correct location (#662)
  * Use relative imports in `speaksfor_util` if possible. (#657)
  * Fix URL to URN lookups to better handle names that differ by a prefix. (#683)
  * Increase the sleep between busy SSL call retries from 15 to 20 seconds. (#697)
  * Rename `addAliases.sh` to `addAliases.command` for Mac install. (#647)
  * Add new section `omni_defaults` to the Omni config file. (#713)
    * This should set system defaults. Where these overlap in meaning with other 
      omni_config or commandline options, let those take precedence.
    * Allow these `omni_defaults` to be specified in the `agg_nick_cache`. These
      are read before those in the per-user omni config. If a default is set
      in the `agg_nick_cache`, that takes precedence over any value in 
      the user's `omni_config`: if a user should be able to over-ride the 
      default, use a different omni config setting (not in `omni_defaults`), or
      use a command line option.
    * Stitcher uses this for the SCS URL.
  * Allow FOAM/AL2S AMs to submit sliver URNs that are the slice URN with an ID 
    appended. 
    * This works around known bug http://groups.geni.net/geni/ticket/1294. (#719)
  * Work around malformed ION sliver URNs: (#722)
    * Allow submitting URNs that use the slice authority as the sliver authority.
    * If the sliver urn reported by sliverstatus is malformed, replace it
      with the proper URN as the manifest returns, so sliver_info reporting
      works without even deleting the existing entry.
    * See http://groups.geni.net/geni/ticket/1292
  * Quiet down some debug logs when printing SSH keys and when talking to CHAPI. (#727)

 * Stitcher
  * Stitcher can now process any multi aggregate bound request. Any request
    with multiple nodes bound to multiple aggregates can be handled directly by stitcher,
    with stitcher calling `createsliver` on each aggregate. Only bound requests
    can be handled though. (#670)
    * Combined manifest is now named: `<slicename>-manifest-rspec-multiam-combined.xml`
  * Added new option `--useSCSSugg` to always use VLAN tags picked
    by the SCS, not letting the AM pick. (#675)
    * Refactor the picking 'any' VLAN tag code into another method,
      and print the INFO message only once. (#705)
  * Add new option `--includehoponpath <hop> <path>` for requiring
    a particular hop only on a particular path, not all paths. (#654)
  * If you have multiple ExoGENI AMs in your request, you must submit
    the request to the ExoSM. Automatically do this. (#689)
  * If multiple aggregates claim to be the ExoSM (due to `--useExoSM` or the above
    work), then merge these together so we only call the ExoSM once. (#688)
    * When doing so, merge `dependsOn` and `isDependencyFor`. (#734)
  * If a link only includes aggregates whose name contains 'exogeni',
    then don't mark the RSpec as needing the SCS. We can use ExoGENI stitching.
    However, if you supply the new option `--noEGStitching` then call the
    SCS anyhow to try to use GENI stitching (which may cause stitcher to fail
    if the SCS does not know of a GENI stitching path between the aggregates).
    Mixing links between ExoGENI AMs and stitched links involving non-ExoGENI AMs 
    remains problematic therefore, as the SCS will fail if it can't find
    every path requested. (#692)
  * Help experimenters avoid links that unexpectedly expire, and avoid
    links that continue past the expiration of compute nodes. (#577)
    * Improved reporting of actual sliver expirations when the run is complete.
    * Set the desired sliver expiration in the request RSpec based on
      current (September, 2014) aggregate policies. InstaGENI and ION/MAX
      aggregates (at least) honor this request. In practice this means
      that many requests will all expire at the same time. 
    * Note that some Utah aggregates' policy limits new slivers to 5 days, so
      many circuits will all expire in 5 days. Renew your reservation if needed.
    * New aggregate types or new local aggregate policies will require changes
      to the code.
  * "`Insufficient rights`" error is fatal at SFA AMs. (#677)
  * "`Malformed arguments: *** verifygenicred`" is fatal at PG AMs.
    Perhaps the slice needs to be renewed? (#691)
  * "`Malformed rspec`" error is fatal at SFA AMs. (#693)
  * `Duplicate node`" is fatal at PG/IG (2 nodes same `client_id`). (#701)
  * Avoid requesting VLAN tag 'any' at OESS and DCN AMs. (#644)
  * Be sure to avoid requesting 'any' from AMs that do not support it. 
    Copy the `isOESS` attribute across SCS calls. (#681)
  * In `devmode`, keep going if request RSpec is of wrong schema. (#673)
  * Always allow a request RSpec of either PGv2 or GENIv3. (#673)
  * Use relative imports in `scs` if possible. (#658)
  * Loop over all links when adding `component_manager` and `property`
    elements, rather than exit on first. (#680)
  * If a DCN AM reports insufficent bandwidth in sliverstatus, fail. (#653)
  * Handle an OESS VLAN unavailable error. (#696)
  * Check the `-a` arguments for RSpecs bound to a single AM (as
    above, determined by `component_manager_id` on nodes). (#687)
    * Fill it in if missing.
    * Warn if it is more than one AM or differs from what the RSpec specifies.
  * Fix up combining manifests: (#612)
    * Fix bug in finding links to avoid adding stitching links as regular links.
    * Combine all top-level attributes so all namespaces are declared.
    * Handle odd namespace for `schemaLocation`. (#703)
    * Basic support for 2 rspecs with the same node client_id on different AMs. (#700)
    * Merge in any non node/link/stitching top level elements in the combined manifest. (#699)
  * Fix up detection of error codes, specifically when looking at
    `value` in return triple. (#702)
  * If a VLAN was unavailable and another AM picked the tag and full negotiation
    is required, for now fail to the SCS to handle it - even for user requested
    aggregates. (#708)
  * Catch errors XMLifying RSpecs - errors can happen from malformed requests. (#709)
    * When printing errors, print `nodeName` of the element, not `name`. (#723)
  * "`No stitching path`" is fatal at PG. (#710)
  * Avoid treating `value` as a string - it is usually an int. (#711)
  * Set SCS URL from the omni_config entry under `omni_defaults` named `scs_url`
    if present, but over-ride that with any commandline value for `--scsURL`.
    This allows changing the default SCS location without updating Omni. (#713)
  * "`such a short life for a sliver`" is fatal at PG. (#714)
  * "`Error encountered converting RSpec to NDL`" is fatal at EG. (#715)
  * "`Embedding workflow ERROR` is fatal at EG. (#716)
  * Fix copying attributes for later tries when the URN changed,
    like switching from an EG rack to the ExoSM. (#718)
    * This bug causes requesting VLAN `any` from EG AMs.
  * For OESS/AL2S AM: Force a call to `sliverstatus` after reservations,
    so slivers are reported to the Clearinghouse. Works around 
    known AL2S issue http://groups.geni.net/geni/ticket/1295. (#717)
  * Re-calculate ready aggregates when launcher catches an error. (#720)
  * ProtoGENI needs the available ranges to all exclude any failed tags
    at all hops at the AM or that face the AM. In particular when a DCN aggregate
    gives a VLAN PCE error. Work around this shortcoming. (#721)
  * Generic `Exception` from OESS is fatal. (#728)
  * If an upstream AM picked a tag that is unavailable locally: (#725)
    * Mark local unavailable tags as unavailable upstream.
    * Check all hops to make such corrections, and then retry at the SCS.
    * Exclude local unavailable tags from the upstream request range, bail to the SCS,
    * Before doing allocations, check hops that do not import VLANs for errors:
      * Range requested must exclude unavailable tags.
      * Hops that import from this hop must have their tags marked unavailable here, 
      and the tag range here must exclude tags marked unavailable there.
      * Bug fix in this logic to handle hops at AMs that don't support 'any'. (#732)
  * Fix regular expression for finding circuit IDs in sliver URNs to handle more variety.
  * Rework logic for when a hop that imports vlans means we cannot
    redo the request locally (#726)
    * Logic comparing the hop to failedHop was buggy, causing the code
      to try to handle locally cases that cannot be handled locally.
      (Like a VLAN unavailable at AL2S.)
    * Make sure the local hop's unavailable tags includes any failed tag.
    * Make sure the hop this imports from's unavailable tags includes the failed
      tag.
    * AL2S style failure where no specific hop failed and hops import is still a case we
      cannot redo locally.
      * The tag selected on such hops should be marked unavailable where we
        import from.
  * Handle new VLAN unavailable error message from OESS. (#729)
    * And properly parse it. (#737)
  * Handle malformed status from ION where 1 circuit failed but overall status is ready. (#731)
  * When editing down the available range, exit if the range is now empty. (#733)
  * Exit from launcher if 0 ready aggregates but not done - that's a bug. (#735)
  * Ensure `inProcess` is false when we retry. (#736)
  * Clarify some error messages: (#741)
    * `Topology too complex` means we need to go to the SCS to find a VLAN, but will retry.
    * ExoGENI `Embedding workflow ERROR` can mean no more VLAN tags. 
  * When reading an alternate URL using the agg_nick_cache, use the proper URL not the 
    version without the http/https prefix used for matching. (#740)
  * Exit if an EG AM is in maintenance (#742)

 * readyToLogin
  * Handle when omni switches AM API version number (#643)
  * Remove defunct code (#666)
  * Remove reliance on 'pg_manifest' field of SliverStatus return.  Leave fallback code for now. (#665)
  * Replace `--noFallbackToStatusForPG` with `--fallbackToStatusForPG` with the opposite meaning (#393)
  * Handle the same client_ids at different aggregates (#669)
  * Handle errors from AMs getting the manifest using APIv3. (#707)
    * Better error detection.
    * Also, use cached GetVersion information if available.

 * remote-execute
  * Allow `--host` to be specified multiple times (#668)

 * clear-passphrases
  * Use SFA libraries to remove passphrases, removing reliance on OpenSSL binary.
    Better error handling and cleaner log messages. 
    Use `oscript` for config file loading. (#664)

 * gcf
   * Return `geni-expires` from the AM API v2 AM. (#679)

# Prior Releases
For notes on earlier releases, see the (CHANGES) file for Github.
