# NYCIST Mac Admin Tools

This repository is to serve as a resource for those who have Macs in their school to share the apps, scripts, and other tools used to manage them.

Some are pulled directly from [https://github.com/timsutton/python-macadmin-tools](https://github.com/timsutton/python-macadmin-tools).

## Index

- [OS Building & Installer Tools](#os-building--installer-tools)
- [Configuration Profiles](#configuration-profiles)
- [Remote Management & Reporting](#remote-management--reporting)
- [Software Management, Packaging, Deployment, & Updates](#software-management-packaging-deployment--updates)
  - [Munki](#munki)
- [Jamf-Specific Tools](#jamf-specific-tools)
  - [Jamf Enrollment / Provisioning](#jamf-enrollment--provisioning)
- [Scripts & Other Tools](#scripts--other-tools)
- [Security](#security)

## OS Building & Installer Tools

- **[AutoDMG](https://github.com/MagerValp/AutoDMG)**: macOS app for building a system image suitable for deployment with Imagr, DeployStudio, LANrev, Jamf Pro, and other asr-based imaging tools. While traditional imaging for macOS is dead, is useful for building never-booted OS images with other tools like [vfuse](https://github.com/chilcote/vfuse).
- **[boostrappr](https://github.com/munki/bootstrappr)**: A bare-bones tool to install a set of packages and scripts on a target volume. Typically these would be packages or scripts that "enroll" the machine into your management system; upon reboot these tools would take over and continue the setup and configuration of the machine.  Bootstrappr is designed to be able to run in Recovery mode, allowing you to "bootstrap" a machine fresh out of the box without having to run the Setup Assistant, manually creating a local account, and other unreliable manual tasks. Particularly useful with Macs like the iMac Pro, which does not support NetBoot, and is tricky to get to boot from external media.
- **[BSDPy](https://github.com/bruienne/bsdpy)**: A platform-independent Apple NetBoot (BSDP) service for organizations that have a need for Apple Mac NetBoot functionality but that lack the ability to support OS X Server in order to implement it.
  - Apple has deprecated NetBoot from OS X Server (now just Server) starting with version 5.7.1 per [this KB article](https://support.apple.com/en-us/HT208312).
- **[createinstallmedia](https://support.apple.com/en-us/HT201372)**: Binary command-line tool built into the macOS install application which allows you to create bootable macOS installers on a USB thumbdrive or other external media.
- **startosinstall**: Binary command-line tool built into the macOS install application which allows you to remotely wipe and install a fresh copy of macOS. This is Apple's supported way of accomplishing the traditional imaging step of installing macOS. Can be used inconjunction with munki or Jamf to achieve a single-button macOS install workflow.
  - [Reinstall a clean macOS with one button with Jamf (article)](https://www.jamf.com/blog/reinstall-a-clean-macos-with-one-button/)
  - [Performing an Erase and Install with jamJAR / munki ](https://dazwallace.wordpress.com/2018/10/11/performing-an-erase-and-install-of-mojave-with-jamjar/)
- **[EraseInstall](https://marketplace.jamf.com/details/eraseinstall/)**: macOS app wrapper for the `startosinstall` tool to allow users to easily erase the HD on their Mac and install a fresh copy of macOS.
- **[erase-install.sh](https://github.com/grahampugh/erase-install)**: Shell script to automate Apple's `--eraseinstall` method to reinstall macOS. Utilizes the `startosinstall` binary, which is built into macOS installer applications since version 10.13.4. If run without any options, the script will not perform the erase. This means that the script can also be used to pre-cache the installer, or simply to make it available for the user.
- **[installinstallmacos.py](https://github.com/munki/macadmin-scripts#installinstallmacospy)**: A Python script that can create disk images containing macOS installer applications available via Apple's softwareupdate catalogs.
  - Built in, macOS Catalina can accomplish something similar: `softwareupdate --fetch-full-installer --full-installer-version 10.1X.X`
- **[Mac Deploy Stick (MDS)](https://twocanoes.com/products/mac/mac-deploy-stick/)**: Free, open-source macOS app to create a USB flash drive for recovering / mass deploying Macs.
  - MDS also offers a [$50 MDS Automaton](https://store.twocanoes.com/collections/frontpage/products/mac-deploy-stick-bundle) for automating the commands to start up in the recovery partition and kick off workflows, making it even easier to setup and deploy a large number of Macs.
- **[System Image Utility](https://support.apple.com/guide/system-image-utility/welcome/mac)**: Built-in macOS app (`/System/Library/CoreServices/Applications/System Image Utility.app`) which can convert a macOS install application into a NetInstall / NetBoot image for network installations.
- **[vfuse](https://github.com/chilcote/vfuse)**: Script that takes a never-booted DMG and converts it into a VMware Fusion VM. Very useful for testing DEP enrollment and other tasks.

[Back to top](#index)

## Configuration Profiles

- **[Extinguish](https://github.com/arubdesu/Extinguish)**: Generates configuration profiles to turn off individual Sparkle-based auto updater apps by default.
- **[iMazing Profile Editor](https://imazing.com/profile-editor)**: macOS app for building and editing custom configuration profiles, developed by DigiDNA. Utilizes the same [open-source framework](https://github.com/ProfileCreator/ProfileManifests) as [ProfileCreator](https://github.com/ProfileCreator/ProfileCreator) and is under active development.
- **[PPPC Utility](https://github.com/jamf/pppc-utility)**: A macOS GUI app for more easily creating PPPC payloads in Jamf. Once created in the app, can be uploaded directly into a Jamf Pro or Jamf Cloud instance.
- **[ProfileCreator](https://github.com/ProfileCreator/ProfileCreator)**: macOS app for building and editing custom configuration profiles for managing preferences. [Extensive wiki](https://github.com/ProfileCreator/ProfileCreator/wiki), supports many populart third-party preferences, and has an [open-source framework](https://github.com/ProfileCreator/ProfileManifests) which is actively contributed to with new and updated profile payloads and preferences.

[Back to top](#index)

## Remote Management & Reporting

- **[Apple Remote Desktop](https://apps.apple.com/us/app/apple-remote-desktop/id409907375?mt=12)**: Paid macOS app for remotely viewing and controlling other Macs.
- **[Microsoft Remote Desktop](https://apps.apple.com/us/app/microsoft-remote-desktop-10/id1295203466?mt=12)**: macOS app for connecting to a remote virtual or physical PC.
- **[munki-facts](https://github.com/munki/munki-facts)**: A Python script and framework for "admin-provided conditionals" for munki. Similar to the function of extension attributes in Jamf.
- **[MunkiReport](https://github.com/munkireport/munkireport-php)**: An open-source reporting client for [munki](https://github.com/munki/munki).
- **[osquery](https://www.osquery.io/)**: Open-source tool that utilizes basic SQL commands to leverage a relational data-model to describe a device. Incredibly performant, a single SQL command can query the same information, whether the device is running Linux, Mac, or Windows. Originally developed by Facebook, control has transitioned to the [Linux Foundation](https://www.linuxfoundation.org/press-release/2019/06/the-linux-foundation-announces-intent-to-form-new-foundation-to-support-osquery-community/).
- **[Sal](https://github.com/salopensource/sal)**: A Django-based reporting app for [munki](https://github.com/munki/munki), integrates with Facter facts on clients.

[Back to top](#index)

## Software Management, Packaging, Deployment, & Updates

- **[appleloops](https://github.com/carlashley/appleLoops)**: Python script for downloading and deploying essential and optional audio content for GarageBand, Logic Pro X, and MainStage 3.
- **[Apple Remote Desktop](https://apps.apple.com/us/app/apple-remote-desktop/id409907375?mt=12) ($)**: Paid macOS app for remotely viewing and controlling other Macs. Can also remotely install PKGs and run scripts.
- **[autopkg](https://github.com/autopkg/autopkg)**: AutoPkg is an automation framework for macOS software packaging and distribution, oriented towards the tasks one would normally perform manually to prepare third-party software for mass deployment to managed clients.
- **[AutoPkgr](https://github.com/lindegroup/autopkgr)**: A macOS app which provides a GUI to install and manage [autopkg](https://github.com/autopkg/autopkg).
- **[chrome-disable-autoupdates.py](https://github.com/hjuutilainen/adminscripts/blob/master/chrome-disable-autoupdates.py)**: Python script for disabling automatic Chrome browser updates for all users on a Mac. It can optionally remove the whole Keystone and its ticket store too.
  - Chrome updates can also be managed by configuration profile. See [ProfileCreator's](https://github.com/ProfileCreator/ProfileCreator) `Google Software Updates` payload for configuring Chrome and/or all Google app update settings.
- **[chrome-enable-autoupdates.py](https://github.com/hjuutilainen/adminscripts/blob/master/chrome-enable-autoupdates.py)**: Python script for enabling automatic Chrome browser updates for all users on a Mac.
  - Chrome updates can also be managed by configuration profile. See [ProfileCreator's](https://github.com/ProfileCreator/ProfileCreator) `Google Software Updates` payload for configuring Chrome and/or all Google app update settings.
- **[iMazing](https://imazing.com/) ($)**: Trusted software to transfer and save your music, messages, files and data. Safely back up any iPhone, iPad or iPod touch. Powerful and user-friendly, iMazing is simply the best iOS device manager for Mac and PC. Get full control over your iOS device. Get iMazing. Integrates with iMazing Profile Editor.
  - **[iMazing Configurator](https://imazing.com/configurator) ($)**: Enterprise component of iMazing that builds on the features of Apple Configurator 2 to help sys admins configure and provision fleets of Apple mobile devices, including iPhone, iPad, iPod touch and Apple TV.
- **[Installomator](https://github.com/scriptingosx/Installomator)**: Described as the 'one installer script to rule them all', for environments who do not have (or do not want to have) a centralized software management system managed in house, this script is meant to allow the download and install of software that is simple, extensible, independent of any specific management system, and which requires no additional dependencies. Works with PKGs, drag-and-drop apps in DMGs, and ZIPs. [More info here](https://github.com/scriptingosx/Installomator#goals).
- **[margarita](https://github.com/jessepeterson/margarita)**: Margarita is a web interface to reposado the Apple Software Update replication and catalog management tool. While the reposado command line administration tools work great for folks who are comfortable in that environment something a little more accessible might be desired.
- **[munki-pkg](https://github.com/munki/munki-pkg)**: Tool for building packages in a consistent, repeatable manner from source files and scripts in a project directory.
- **[nudge](https://github.com/macadmins/nudge)**: A tool to help users with pre-existing devices upgrade their OS version. Merely *nudges* users to install Apple software updates via an approved method (System Preferences, Munki, Jamf, etc.).
- **[RecipeBuilder](https://github.com/mikaellofgren/RecipeBuilder)**: macOS app (10.14+) for more easily writing [autopkg](https://github.com/autopkg/autopkg) recipes.
- **[Recipe Robot](https://github.com/homebysix/recipe-robot)**: macOS app for automatically creating recipes for [autopkg](https://github.com/autopkg/autopkg).
- **[reposado](https://github.com/wdas/reposado)**: Reposado is a set of tools written in Python that replicate the key functionality of Mac OS X Server's Software Update Service, allowing you to host Apple Software Updates on the hardware and OS of your choice.
  - Reposado & margarita in Docker: [sphen/reposado](https://hub.docker.com/r/sphen/reposado)
- **[SD Notary](https://latenightsw.com/sd-notary-notarizing-made-easy/)**: a macOS app for more easily handling the process of submitting .app, .pkg, .dmg, and .fmplugin for notarization by Apple. Requires Xcode and the commandline tools to be installed, as well as having an Apple ID in the Apple Developer program. [Download here](https://s3.amazonaws.com/latenightsw.com/SDNotary1.4.1-44.zip).
- **softwareupdate**: Built-in macOS command-line tool for managing software updates. Utilized by tools like [munki](https://github.com/munki/munki/) and Jamf.
- **[Suspicious Package](https://www.mothersruin.com/software/SuspiciousPackage/)**: macOS app for inspecting the contents of PKG installers. See if it's signed and by whom, the files that are installed, versions and other metadata, as well as the scripts it may run. You can also export individual items from a PKG to your Mac for closer inspection.
- **[SUS Inspector](https://github.com/hjuutilainen/sus-inspector)**: Short for **S**oftware **U**pdate **S**ervice Inspector, a macOS utility app for viewing detailed information about Apple's Software Update Service. It sets up a local [reposado](https://github.com/wdas/reposado) installation to replicate catalogs and then parses them for viewing. Can be used in combination with [MunkiAdmin](https://github.com/hjuutilainen/munkiadmin) to create files for [munki](https://github.com/munki/munki) to handle these installations.

[Back to top](#index)

### Munki

- **[jamJAR](https://github.com/dataJAR/jamJAR)**: jamJAR is the glue that takes the best about [munki](https://github.com/munki/munki) in terms of software deployment and allows you to use it with Jamf. You must have both a Jamf and a [munki](https://github.com/munki/munki) to use.
  - jamJAR is a solution that applies out of the box thinking & lean methodologies to macOS "patch management". This holistic approach synergises Jamf, [autopkg](https://github.com/autopkg/autopkg) & [munki](https://github.com/munki/munki) into an aggregated convergence that cherry-picks functionality from each products core competency to create an innovative, scalable & modular update framework.
- **[munki](https://github.com/munki/munki)**: An open-source project with a set of tools that when used together with a web server, can be used to manage software installs and removals for macOS.
- **[munki-rebrand](https://github.com/ox-it/munki-rebrand)**: Scripts to rebrand munki's Managed Software Update center.
- **[MunkiAdmin](https://github.com/hjuutilainen/munkiadmin)**: MunkiAdmin is a macOS app for managing [munki](https://github.com/munki/munki) repositories via a GUI.
- **[PrinterGenerator](https://github.com/nmcspadden/PrinterGenerator)**: Script for generating specific 'nopkg' pkginfo files for printer configurations.

[Back to top](#index)

## Jamf-Specific Tools

- **[cachecheck](https://github.com/krypted/cachecheck)**: Extension attribute to check which Caching Server a Mac is using.
- **[CommunityPatch](https://github.com/brysontyrrell/CommunityPatch)**: CommunityPatch is a free, open-source, external patch source for Jamf Pro administrators to publish patch definitions they maintain for the broader Jamf community to subscribe to. Access CommunityPatch using an Apple ID, and then create and manage API tokens to interact with the service API.
- **[git2jss](https://github.com/gkluoe/git2jss)**: Python script to bring your Jamf Pro server scripts and extension attributes into version/change control. When changes are committed, can sync the changed scripts and EAs back into Jamf.
- **[Jackalope Slack](https://marketplace.jamf.com/details/jackalope-slack/)**: A Slack app to receive notifications on events from your Jamf Pro Server to any number of Slack channels on your Slack Team.
- **[jamf2snipe](https://github.com/ParadoxGuitarist/jamf2snipe)**: Python script for syncing Macs from a Jamf Pro instance to a [Snipe-IT](https://snipeitapp.com/) asset management instance.
- **[Jamf Connect](https://www.jamf.com/products/jamf-connect/)**: Additional paid Jamf product born from purchasing the company behind [NoMAD](https://nomad.menu/products/#nomad) and [NoMAD Login](https://nomad.menu/products/#login). Allows the use of cloud-identity credentials at the macOS loginwindow.
- **[Jamf Environment Test (JET)](https://github.com/jamf/Jamf-Environment-Test)**: macOS app utility for testing an environment for success with Apple Devices. This tool is designed to be used by an Apple admin in conjuction with a networking admin on a network that the Apple device will be on. Reports on current device proxy settings, and its connectivity to Apple URLs [listed here](https://support.apple.com/en-us/HT210060). Bash script available for validation. [Download here](https://github.com/jamf/Jamf-Environment-Test/releases).
- **[JAMF MDMTools.py](https://github.com/rustymyers/jamfMDMTools)**: This python + flask script provides a web service to easily send wipe and lock commands to macOS devices enrolled in your Jamf server. It lists all systems that are enrolled and their MDM status. Checking the box next to them allows you to send a wipe, or lock, with a custom six digit passcode. The tool was written to provide a simple method to send wipe commands to many systems at once.
- **[JamfProSwitcher](https://github.com/ninxsoft/JamfProSwitcher)**:  A macOS app utility that quickly switches between multiple Jamf Pro Server instances.
- **[Jamf Migrator](https://github.com/jamf/JamfMigrator)**: macOS app for migrating data granularly between Jamf Pro servers. Great for replicating scripts, profiles, and other data between Jamf Pro servers. [Download here](https://github.com/jamf/JamfMigrator/releases).
- **[Jamf Reset](https://marketplace.jamf.com/details/jamf-reset/)**: iOS app to streamline the workflow of resetting an iOS device in a shared iOS device environment. Used in conjunction with [Jamf Setup](https://marketplace.jamf.com/details/jamf-setup/).
- **[Jamf Setup](https://marketplace.jamf.com/details/jamf-setup/)**: iOS app to enable the automatic configuration of an iOS device in a shared iOS device environment. Used in conjunction with [Jamf Reset](https://marketplace.jamf.com/details/jamf-reset/).
- **[jHelper GUI](https://github.com/jamf/jHelper-GUI)**: A macOS app for creating 'Jamf Helper' command line arguments.
- **[jss_helper](https://github.com/jssimporter/jss_helper)**: A powerful commandline interface for managing and auditing your Jamf Pro Server.
- **[JSSImporter](https://github.com/jssimporter/JSSImporter)**: A framework for connecting software downloaded via autopkg to a Jamf Pro server. Will import the software to a Jamf distribution point, as well as create an applicable policy and scope for deployment.
- **[JSSRecipeCreator](https://github.com/jssimporter/JSSRecipeCreator)**: If you use [autopkg](https://github.com/autopkg/autopkg), this script allows one to rapidly make `.jss` recipes based on a set of template files.
- **[JSS Switcher](https://github.com/dataJAR/Jamf-Switcher)**: A macOS app which points either Jamf Pro applications or your default browser to a particular Jamf deployment. Configured by Self Service Bookmarks.
- **[SCL Jamf Tools](https://github.com/univ-of-utah-marriott-library-apple/scl_jamf_tools)**: Github repository with 2 macOS apps for enhancing Jamf Pro management software developed by the University of Utah, Marriott Library.
  - **[Cargo Ship](https://github.com/univ-of-utah-marriott-library-apple/scl_jamf_tools#cargo-ship)**: macOS app for reading directly from a JSS the configuration of individual Macs including scoped policies and smart groups, installed printers and packages, configuration profiles, and extension attribute values.
  - **[Tugboat](https://github.com/univ-of-utah-marriott-library-apple/scl_jamf_tools#tugboat)**: macOS app for making modifications to specific Mac records.
- **[Spruce](https://github.com/jssimporter/Spruce)**: Python script for identifying unused packages, scripts, and policies on a Jamf Pro Server and optionally remove them. Can also run a number of device reports as well.
- **[The MUT](https://github.com/mike-levenick/mut)**: The unofficial, all-in-one mass update tool designed to be the perfect companion to Jamf Admins. A native macOS app which allows Jamf Admins to make mass updates to attributes (such as username, asset tag, or extension attribute) of their devices and users in Jamf.

[Back to top](#index)

### Jamf Enrollment / Provisioning

- **[DEPNotify](https://marketplace.jamf.com/details/depnotify/)**: DEPNotify runs as a native macOS window and can be easily controlled via a text file. If asked it will automatically show all policies and installations done by Jamf as they happen.
- **[JAMF Enrollment Kickstart](https://github.com/Yohan460/JAMF-Enrollment-Kickstart)**: A better enrollment kickoff for JAMF machines. Jamf's native `enrollmentContinue` trigger is generally unreliable, as it can be interrupted by a number of different things. This tool gives you a reliable mechanism for carrying out the things you want to have happen immediately following enrollment into your Jamf Pro Server.
- **[msupdatehelper](https://github.com/pbowden-msft/msupdatehelper)**: Command-line tool for updating Microsoft Office apps through Jamf. Training video on how to use this tool: [http://office4mac.com/courses/msupdate](http://office4mac.com/courses/msupdate)
- **[SplashBuddy](https://github.com/Shufflepuck/SplashBuddy)**: Onboarding splash screen for Jamf Pro to help improve the DEP provisioning process on Macs by providing an elegant and secure process for users.

[Back to top](#index)

## Scripts & Other Tools

- **[AirServer](https://www.airserver.com/)**: macOS app for turning a Mac into a screen mirroring receiver similar to an Apple TV. Goes further in that it supports both AirPlay and Chromecast, as well as multiple simultaneous device mirroring.
- **[Asset Catalog Tinkerer](https://github.com/insidegui/AssetCatalogTinkerer)**: A macOS app that lets you open .car files within apps built with Xcode to browse and/or extract their images. Useful when you want to use an image/icon users will recognize.
- **[Atom](https://atom.io/)**: A free, hackable text editor with many installable packages.
- **[BBEdit](https://www.barebones.com/products/bbedit/index.html)**: Another text editor. Can acquire a free EDU license for your school to avoid the 30-day pay timer.
- **[Consolation](https://eclecticlight.co/consolation-t2m2-and-log-utilities/)**: macOS app, provides an accessible but powerful way to browse, search, and analyse entries in the new macOS log system which have already been captured that is not supported by Apple's Console app. If you want to check that Time Machine backups have been made on time and without error, or get to the bottom of startup, extension, or many other problems, Consolation is the only practical tool to use.
- **[defaultbrowser](https://github.com/kerma/defaultbrowser)**: Command-line tool for setting the default browser. Similar to [SwiftDefaultApps](https://github.com/Lord-Kamina/SwiftDefaultApps), but more limited.
- **[desktoppr](https://github.com/scriptingosx/desktoppr)**: Command-line tool for setting the Desktop background.
- **[DetectX Swift](https://sqwarq.com/detectx/)**: A lightweight macOS app for completing on-demand search and troubleshooting tool. that can identify malware, adware, keyloggers, potentially unwanted apps and potentially destabilising apps on a Mac. *VERY* inexpensive [management license](https://sites.fastspring.com/sqwarq/product/detectxswiftmanagement), and can be triggered remotely to complete full system scans.
- **[Docker](https://www.docker.com/)**: Docker is a tool designed to make it easier to create, deploy, and run applications by using containers. Containers allow a developer to package up an application with all of the parts it needs, such as libraries and other dependencies, and ship it all out as one package. By doing so, thanks to the container, the developer can rest assured that the application will run on any other Linux machine regardless of any customized settings that machine might have that could differ from the machine used for writing and testing the code ([https://opensource.com/resources/what-docker](https://opensource.com/resources/what-docker)). Great for testing.
  - [podman](https://podman.io/): An alternative to Docker.
- **[dockutil](https://github.com/kcrawford/dockutil)**: Tool for programmatically configuring Docks. Can be used in conjunction with other tools, like [outset](https://github.com/chilcote/outset).
- **[docklib](https://github.com/homebysix/docklib)**: Another tool for programmatically configuring Docks. Can be used in conjunction with other tools, like [outset](https://github.com/chilcote/outset).
- **[Hancock](https://github.com/JeremyAgost/Hancock)**: Hancock is a GUI tool for signing packages and mobileconfig files. First it looks through your keychain for all certificates that can be used to sign, then signs the files using the selected certificate.
- **[mysides](https://github.com/mosen/mysides)**: Command-line tool for programmatically configuring the Finder sidebar.
- **[NoMAD](https://nomad.menu/products/#nomad)**: Open-source macOS app to help users bound to Active Directory, its main purpose is to help move your Macs off binding to AD while still getting all of the functionality. Keep your users on local accounts and let NoMAD manage their interaction with AD by allowing them to sign in with their AD account to get Kerberos tickets, certificates for 802.1X connections and other functions without having to have a mobile account.
- **[NoMAD Login](https://nomad.menu/products/#login)**: Open-source macOS companion app to [NoMAD](https://nomad.menu/products/#nomad) for allowing AD logins at the loginwindow without needing to bind to AD.
- **[offset](https://github.com/aysiu/offset)**: Similar to [outset](https://github.com/chilcote/outset), but automatically processes packages and scripts at logout.
  - Reportedly does not work with macOS Catalina ...
- **[Outlook-Exchange-Setup](https://github.com/talkingmoose/Outlook-Exchange-Setup-5)**: Script that provides your volume-licensed Outlook for Mac users with automatic setups of their Exchange accounts. It works especially well if the Mac is bound to Active Directory.
- **[outset](https://github.com/chilcote/outset)**: Automatically process packages, profiles, and scripts during boot, login, or on demand as the logging-in user or with administrator privileges.
- **[Push Diagnostics](https://twocanoes.com/products/mac/push-diagnostics/)**: A macOS app that will quickly verify that the appropriate hosts can be reached on their respective ports from any network you are on.
- **[ripgrep](https://github.com/BurntSushi/ripgrep)**: Recursively search directories for a regex pattern. Searches both filenames and file contents for your pattern, excluding hidden files, directories, and binary files. *Very* performant and super useful!
- **[Show Me Your ID](https://www.hcsonline.com/support/apps/show-me-your-id)**: macOS app that will provide you with the Bundle ID, Team ID, and the full code signature for an application by dragging the application to the Show Me Your ID window. Useful when building PPPC profiles.
- **[Suitcase](https://github.com/Impedimenta/Suitcase)**: A flexible command line tool for instantly deploying user interfaces (SwiftUI) for simple commands and scripts.
- **[SwiftDefaultApps](https://github.com/Lord-Kamina/SwiftDefaultApps)**: Command-line tool and (optional) preference pane for configuring the default application associations for basically any URI Scheme and/or filetype in macOS (ex. open .csv files with Excel instead of Numbers).
- **[Taccy](https://eclecticlight.co/taccy-signet-precize-alifix-utiutility-alisma/)**: A macOS app for reading the TCC / PPPC requirements of other macOS apps. Avoids needing to determine by hand what permissions (if any) are needed by apps you deploy.
- **[tccdbRead.py](https://github.com/carlashley/tccprofile/blob/master/tccdbRead.py)**: Python script for reading system and user-level TCC database to determine what Privacy Preferences Policy Control (PPPC) permissions have been manually allowed.
- **[Ulbow](https://eclecticlight.co/consolation-t2m2-and-log-utilities/)**: macOS app,  simplest browser for the macOS Unified Log, without losing any of the power of [Consolation](https://eclecticlight.co/consolation-t2m2-and-log-utilities/). Ideal for the casual user as well as log addicts.
- **[VSCodium](https://github.com/VSCodium/vscodium)**: Open-source version of Microsoft's VSCode without their branding, telemetry, and licensing.
- **[Zentral](https://github.com/zentralopensource/zentral)**: An open-source hub for gathering, processing, and monitoring system events and linking them to an inventory. Uses Elastic Search and supports extensions for many popular agents (munki, osquery, Santa, etc.) and inventory systems (Filewave, Jamf, etc.).

[Back to top](#index)

## Security

- **[Crypt](https://github.com/grahamgilbert/crypt)**: Crypt is an authorization plugin that will enforce FileVault 2, and then submit it to an instance of [Crypt Server](https://github.com/grahamgilbert/crypt-server).
- **[Escrow Buddy](https://github.com/macadmins/escrow-buddy/)**: A framework for re-escrowing missing or invalid FileVault keys with Jamf Pro.
- **[Jamf Protect](https://www.jamf.com/products/jamf-protect/)**: Jamf product that utilizes the Mac's built-in security tools, namely Apple's new Endpoint Security framework for analyzing macOS system events on device to streamline security insights.
- **[KeePassXC](https://keepassxc.org/)**: An open-source, free, cross-platform password manager.
- **[LAPSforMac](https://github.com/NU-ITS/LAPSforMac)**: An open-source shell script to complement Microsoft LAPS. Securely manages and rotates the password of a local admin account and stores this information within a Jamf Pro extension attribute.
- **[OSXCollector](https://github.com/Yelp/osxcollector)**: A forensic evidence collection & analysis toolkit for macOS, developed by Yelp.
- **[Privileges](https://github.com/SAP/macOS-enterprise-privileges)**: macOS app developed by SAP for allowing standard user accounts to obtain administrator rights for a set period of time. Manageable via configuration profile.
- **[Santa](https://github.com/google/santa)**: A binary whitelisting/blacklisting system for macOS. Can block based on file hash, signing certificate, and/or path. Can also be managed via configuration profile or central [sync server](https://github.com/google/santa#sync-servers).
- **[U. of Utah Marriott Library Firmware Password Manager](https://github.com/univ-of-utah-marriott-library-apple/firmware_password_manager)**: Python script to automate the management of firmware / EFI passwords.

[Back to top](#index)
