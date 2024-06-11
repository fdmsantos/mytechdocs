# Azure Virtual Desktop

## Products

* AVD Service
  * PaaS Remote Desktop Solution Built in Azure
* Window 10 Multi-user
  * Windows 10 OS that allows multiple users sessions
  * Only Available in Azure
* FSLogix
  * User profile solution
  * Provides a consistent user experience between multiple sessions
* MSIX App Attach
  * Containerized version of applications attach to a client OS

## Terms

* Session Host
  * One or more computers that host remote desktop and/or remote application sessions
* Host Pool
  * A group of Session Hosts. Session Hosts in a host poll should be sized and configured identically
* Remote Application
  * An application available to the end user. An application installed on the session hosts or a full desktop can be published as a remote application
* RemoteApp Application group
  * A group of one or more remote applications. A Host Pool can hava multiple RemoteApp Application Groups.
  * Users are assigned permissions to application groups
* Desktop Application Group
  * A published remote desktop
  * A host pool can only have one Desktop Type
  * Application Groups and one is created by default
* Workspace
  * A logical grouping of application groups users interact with a workspace

## Host Pool

* Types
  * Personal Host Pool
    * One-to-one mapping of user to vm windows 7 or windows 10
    * Uses Cases:
      * Windows 7
      * High Processing Apps, Like GPU, Analytics, Graphic processing
  * Pooled Host Pool
    * Multiple users sessions per VM
    * Windows Server 2012 R2, 2016, 2019 or Windows Multi User
    * Load Balancing
      * Bread-first
        * connections distributed across all available session hosts
      * Depth-first
        * connections consolidated on one session host until the maximum connections is reached