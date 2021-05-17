## Roles

The AppGuard Management System supports both Workstation and Windows
Servers Roles, as well as a Linux Server role. AppGuard provides
different policies for each of these roles because the attack surfaces
are different depending on the computer role:

-   **Windows Workstation Role**: This policy should be used for
    desktops and laptops. Supported workstations are Windows 7, Windows
    8.1 and Windows 10.

-   **Windows Locked Down Workstation Role**: Administrators can follow
    our step-by-step guide to create a *locked down* role based on the
    existing Windows Workstation role.

-   **Windows Server Roles**: Each of the server roles contain
    protections for both IIS and SQL Server. Even though your server may
    not be used for IIS or SQL server roles, these roles can be used.
    There are two types of server roles:

    -   Basic: Use this role policy for normal Windows server
        configurations. This role policy includes the "best practice"
        default settings to secure normal Windows server operations.

    -   Locked Down: This policy locks down a server such that only the
        mission critical features function. Maintenance tasks require
        that AppGuard be turned off.

The Role Policy is defined in the Administration Section under "Role
Policy List". Each of the role policies provided can be modified and
additional role policies can be created by the administrators. Each of
the built-in roles are described in more detail in the sections below:

### Windows Workstation

Workstations are primarily attacked by exploiting a 0-day vulnerability
in an application that interfaces to the internet (browsers and mail
clients) or applications that process files that originated from
internet (i.e. Word, Adobe Reader, etc.). Often the attack will leverage
the vulnerability by placing malicious files on the system. AppGuard
prevents these attacks by virtually isolating these highly targeted
applications from critical system components. In addition, AppGuard will
prevent these applications from dropping malicious files onto the system
or prevent these files from running. We call this *Guarding* an
application. The core substance of this policy is defined in the Guarded
Apps and Space rules policy.

AppGuard protects workstations from malware by implementing a set of
pre-launch and post-launch protections that work in concert to prevent
malware from compromising the integrity of the workstation. AppGuard
prevents "untrusted" applications from running and virtually isolates
(or Guards) the "trusted" applications that are most vulnerable to
attack by malware.

For workstations, AppGuard considers applications in *system space* as
trusted, while most apps in *user space* are considered to be untrusted.
System space consists of the areas of the system that normally cannot be
altered by a non-admin end-user (c:\\windows; c:\\Program Files, etc.).
AppGuard also maintains the integrity of system space by preventing the
Guarded apps from altering it. Most apps in system space do not need to
be Guarded. Only those that are most vulnerable to attack (Microsoft
Office, browsers and mail clients) are Guarded.

User space consists of those areas of the system that can be altered by
non-admin users --- such as the desktop, downloads and documents
folders. Because user space can be altered by the end-user (and any
program that the end-user runs), AppGuard does not trust apps that are
located in this space, and only allows those apps that are digitally
signed by one of the Trusted Publishers to execute.

**Note:** Machines running Linux agents do not recognize Trusted
Publishers. (This concept does not apply to Linux machines.)

The workstation role policy delivered with the AGMS provides a policy
that implements the pre- and post-launch protections described above. To
facilitate creating the workstation role policy, a set of "space rules"
is defined. The following are the primary space rules used by the
workstation role:

-   **US TPL (User Space TPL)**: Only apps listed in the TPL (Trusted
    Publisher List) are permitted to launch. They may be Guarded
    depending on the TPL policy. Guarded applications are permitted to
    write to folders with the US TPL designation.

    **Note:** The TPL concept does not apply to machines running an
    AppGuard Linux agent.

-   **SS Allow All (System Space Allow All)**: All applications are
    permitted to run unless explicitly denied by policy. Guarded
    applications are prohibited from writing to these folders.

These space rules are also used by the workstation policy:

-   **Private US (Private Space rule)**: Only applications listed on the
    TPL are permitted to run. Most guarded applications are permitted to
    write, but applications that are designated as running in "Privacy
    Mode" are not permitted to access this folder.

-   **SS TPL (System Space TPL)**: Only applications listed on the TPL
    are permitted to launch. Guarded applications are not permitted to
    write to this folder.

Additional workstation policies are defined as follows:

-   **Registry**: One of the critical system components is the registry.
    AppGuard prevents the Guarded Apps from accessing important keys in
    the registry. If an application requires access to one of these
    keys, you can add it here. The default workstation role policy does
    not contain any exceptions.

-   **Trusted Publishers (Inapplicable to machines running the AppGuard
    Linux agent)**: AppGuard's "Guarded Apps" policy only permits
    publishers on the Default Trusted Publisher List to execute from
    user space. The Trusted Publisher List policy is also used to
    facilitate auto-upgrade of programs published by a vendor on the
    Trusted Publishers list.

-   **InstallGuard**: Enforces policy related to an end user installing
    software on their endpoint, and is enabled on the View/Edit Group
    (group settings) page. (Refer to *Appendix M - InstallGuard Logic
    (Windows Only)* on page 364, for more information.)

-   **Pass the hash protection**: Pass the hash protection is enforced
    via the Pass the Hash Trusted Enclave policy. Without this
    protection, malware can easily read the memory of one of the key
    windows processes and gain access to user credentials

-   **Power Applications**: Power applications are exempt from AppGuard
    protection. In other words, AppGuard will not interfere with power
    applications. The workstation role contains two power applications
    to facilitate Microsoft Office Software Updates. Third party
    security applications are sometimes added to allow memory inspection
    and unhampered access to the system.

-   **Removable Media**: AppGuard default workstation role policy
    permits access to removable media (USB drives).

-   **Ignore messages**: AppGuard generates multiple types of events
    that are logged and sent to the AGMS server. Ignore messages allows
    you to filter the events that get forwarded by the agent. Filtering
    these at the agent level reduces Administrator and AppGuard
    Management System overhead. There are several pre-configured
    blocking events; these are configured on the Ignore Message policy
    page.

### Windows Server Roles

Common Windows Server Attack vectors are:

-   The SMB (Server Message Block) protocol

-   SQL Server Code Injection

-   Web Application Server Exploits such as DLL Planting Attacks

-   Leveraging of built-in windows utilities and PowerShell

Two Windows Server roles are provided: 1) Basic and 2) Locked down. Both
provide protection against the previously mentioned attack strategies.
The locked down role provides more protection against the leveraging of
built-in windows utilities but might require that AppGuard protections
be disabled in order to perform certain maintenance tasks.

Web Application Server protection is accomplished by a combination of
AppGuard protections: Trusted Enclave, Code Load protection and Guarded
Application protection. These protections are detailed in the sections
that follow.

The server roles implement various Guarded apps and Space rules
policies: In the case of servers, there usually is no user logged on, so
the concepts of a user space divided from a system space does not exist.
The Guarded Apps and Space rules are used to prohibit the risky system
utilities from running. The basic and locked down roles differ in that
with the basic role, some of the utilities are Guarded vs. prohibited.
The details of the application policies can be viewed on the AGMS.

#### Space rules

Three space rules are used in the Server Roles Guarded Apps policy:

1.  Allow only Trusted Publishers; Allow Write: For folders with this
    policy, only trusted publishers applications are permitted to run
    and Guarded applications are permitted to write. (This does not
    apply to machines running an AppGuard Linux agent, since that agent
    does not recognize the trusted publisher concept.)

2.  Allow Applications to Launch; Read Only: All applications are
    permitted to run, but Guarded applications are not permitted to
    write to these folders.

3.  Allow only Trusted Publishers, Read Only: For folders with this
    policy, only trusted publishers applications are permitted to run
    and Guarded applications are not permitted to write to these
    folders. This does not apply to machines running an AppGuard Linux
    agent, since AppGuard Linux does not support the Trusted Publishers
    feature.

#### Trusted Enclaves

Three Trusted Enclaves are defined for the server rules:

1.  Pass the hash protection: Without this protection, malware can
    easily read the memory of one of the key windows files and gain
    access to user credentials. Pass the hash protection is enforced via
    the Pass the Hash trusted enclave policy.

```{=html}
<!-- -->
```
4.  SQL Server Trusted Enclave: This enclave protects against SQL Server
    code injection. It also protects the SQL Server data folders from be
    modified by other applications. Additionally, it protects the rest
    of the system from a SQL Server vulnerability.

5.  IIS Trusted Enclave: This enclave protects the server from malicious
    attack through web application running on the server.

#### Global Policy

Global Policy can prevent any process (even those that aren't Guarded)
from accessing specified folder and registries. The server role policy
uses this protection to prevent processes from accessing registry keys
that aren't normally used during a server's steady state operation, but
that malware often leverages.

#### Code Load Protection

Code load protection is used to prevent suspicious DLLs from being
loaded. The server role policy applies Code Load protection for the
inetpub\\wwwroot folder to protect Web Application Servers from DLL
planting attacks. In the case of the IIS protection, only digitally
signed DLLs are permitted to be loaded. You can optionally limit
authorized code loads to DLLs signed by vendors on the Trusted Publisher
List (but this is inapplicable to machines running AppGuard Linux
agents).

#### Trusted Folder

Though no initial Trusted Folder is specified in the Server Role
Policies, Trusted Folders can be very useful for performing maintenance
tasks. If specified, any PowerShell script or batch file can be executed
from the Trusted Folder without AppGuard interference (even when
PowerShell and cmd.exe are otherwise prohibited). The Trusted Folder is
protected so that only Power Apps may write to the folder. For that
reason, a Trusted Folder's contents cannot be modified while AppGuard is
running.

### Linux Server Role

Common Linux Server attack vectors:

-   Application Vulnerability Exploits

-   Remote code execution from trusted endpoints

-   SQL injection

-   Hijack utilities and administrator user accounts

Linux Server protection is accomplished by a combination of AppGuard
protections: Guarding Applications (contain), Trusted Enclave (isolate
and/or contain), Code Load Protection and. These protections are
detailed in the sections below. The server roles implement both guarded
apps and space rules, as well as trusted enclaves, as detailed in the
following sub-section.

#### Guarded Apps and Space rules

These prohibit risky system utilities from running as well as unknown
scripts/executables from launching from standard Linux file system
locations.

#### Trusted Enclaves

Two Trusted Enclaves are defined:

1.  MySQL: This enclave protects against SQL Server code injection. It
    also protects the SQL Server data folders from be modified by other
    applications. Additionally, it protects the rest of the system from
    a SQL Server vulnerability.

```{=html}
<!-- -->
```
6.  PHP Web Application/Apache: This enclave protects the server from
    malicious attack through web application running on the server.

### Workstation Locked Down Role

The Workstation Locked Down role offers increased security by
blacklisting several Windows features as well as isolating several
important registry keys, though an admin can overcome these limitations
through either (a) invoking a Power App (which, even in this role, is
able to execute, as well as to alter registry keys), or (b) running the
endpoint temporarily in admin mode. Creating a group that uses the
Workstation Locked Down role uses exactly the same steps that creating
any other group does. Besides the changed policy that differentiates the
Workstation Locked Down role from other roles, it was built with
companion security features: NetLock and Privilege Containment. (See
*NetLock* on page 126, as well as *Privilege Containment* on page 130,
for more information.) To see what apps are protected in the Workstation
Locked Down role, do the following:

1.  Navigate to the **Company Overview** of the relevant company.

![](media/image1.png){width="3.776000656167979in"
height="2.462468285214348in"}

2.  From the left navigation, from the **Administration** menu, click
    **Role Policy List**.

![](media/image2.png){width="1.1354330708661418in"
height="2.375999562554681in"}

3.  Click **Workstation - Locked Down**.

![](media/image3.png){width="4.52in" height="1.953837489063867in"}

4.  From the left navigation, click **AppGuard Policies → Default
    Enclave**.

![](media/image4.png){width="1.2727274715660541in"
height="3.075757874015748in"}

7.  To see all the policies for this role, click **Show All**.

![](media/image5.png){width="2.216000656167979in"
height="1.413921697287839in"}

8.  All of the policies for this role appear. In the figure that
    follows, two policies that were created specifically for this role
    are ringed in red.

![](media/image6.png){width="6.5in" height="3.3194444444444446in"}

## System Architecture

The AppGuard system architecture, shown in the figure that follows,
consists of the following components:

-   AGMS (AppGuard Management System), a server application that
    centrally manages the AppGuard enterprise. that communicates with
    PDPs/LRPs (Policy Distribution Points/Log Retrieval Points). The
    AGMS itself consists of a Windows Web server and a SQL server. While
    the AGMS may be installed on a single Windows server (that includes
    both IIS and SQL), the recommended configuration is for the AGMS to
    be on one physical server and its SQL Server to be on a separate
    physical server. Additionally, AppGuard supports clusters of SQL
    Servers for increased scalability and availability.

-   PDPs/LRPs (Policy Distribution Points/Log Retrieval Points) that
    function as a middle communication layer between the AGMS and the
    endpoints. (A PDP and an LRP are normally co-located on the same
    physical server, and therefore the two points are collectively known
    as a PDP/LRP.) The AppGuard enterprise has redundancy through at
    least two PDPs/LRPs, though depending on the deployment size, there
    may be more. LRPs periodically poll the endpoints for the latest
    logs (which show all of the agent activity), and transmit those logs
    to the AGMS for processing and storage. PDPs distribute policies to
    deployed agents, where each agent receives one policy that contains
    a bundle of rules. More specifically, when an admin changes the
    rules for a group of agents, the admin distributes the updated
    policy to PDPs, which then distribute the policy to the individual
    endpoints that are affected. (An admin typically modifies the policy
    of one group of endpoints, not all of the endpoints in a company.)
    Admins may use generic, unsecured servers to distribute AppGuard
    policies, as the policies are encrypted and digitally signed, and
    AppGuard supports FTP, FTPS and REST HTTPS for the communication
    between agents and PDPs, which may require corporate firewall
    adjustments.

-   AppGuard agent, which is cryptographically fortified client software
    that runs on each protected endpoint (and communicates with the
    PDPs/LRPs). The agent on an endpoint is the service that enforces
    AppGuard policy there.

![](media/image7.png){width="5.492142388451444in"
height="3.4919695975503062in"}

## AGMS (AppGuard Management System) Overview

The AGMS (AppGuard Management System), a centralized console used to
administer an AppGuard-protected enterprise, uses a Web interface to:

-   Generate agent install packages.

-   Create policies.

-   Distribute policies to PDPs (Policy Distribution Points).

-   Collect endpoint logs from LRPs (Log Retrieval Points).

-   View log events.

-   Requirements of AGMS and Supporting Components

## AGMS Requirements

The recommended configuration for most deployments is to install the
AGMS on two servers --- a Web Server and a SQL Server, though for
smaller deployments (\<10,000 end points), both components can be
installed on a single server. The requirements vary somewhat from
deployment to deployment, depending on factors such as deployment size,
number of logs generated per endpoint per unit time, and so forth. The
table that follows gives general guidelines that work for many
deployments.

                                          AGMS Web App[^1]                                 AGMS SQL Server                                             LRS                                              PDP/LRP FTP/FTPS                                             PDP/LRP REST
  --------------------------------------- ------------------------------------------------ ----------------------------------------------------------- ------------------------------------------------ ------------------------------------------------------------ ----------------------------------------
  Web Browser                             Internet Explorer, Edge, Firefox, and Chrome                                                                                                                                                                               
  OS Language                             English or Japanese only                                                                                                                                                                                                   
  CPUs                                    4                                                2[^2]/4[^3]                                                 4                                                4                                                            2
  CPU Speed                               3Ghz                                             3Ghz                                                        3Ghz                                             3Ghz                                                         3Ghz
  RAM                                     16GB                                             16GB^2^/24GB^3^                                             8GB                                              2GB                                                          16GB
  Windows Server Version                  x64 Windows Server 2012 R2 and above.            SQL Server (2012, 2016, or 2019), or Azure SQL Service^1^   2012 R2 and above                                2003, 2008 R2 (Requires KB3033929), 2012 R2, 2016, or 2019   2012 R2, 2016, or 2019
  Windows Server Edition                  Standard Edition or Datacenter Edition, 64-bit   Standard Edition or Datacenter Edition, 64-bit              Standard Edition or Datacenter Edition, 64-bit   Standard Edition or Datacenter Edition                       Standard Edition or Datacenter Edition
  Windows IIS                             IIS 6 and above                                                                                              IIS 6 and above                                  IIS 6, IIS 7, and IIS8                                       IIS 6 and above
  .NET Framework                          4.6.1                                                                                                        4.6.1                                                                                                         4.6.1
  VSFTPD                                                                                                                                                                                                FreeBSD, Linux                                               \--
  Native BSD FTPD                                                                                                                                                                                       OpenBSD                                                      \--
  SQL Server Command Line Query Utility   See footnote 1                                   See footnote 1                                                                                                                                                            
  Storage on HD                           200GB                                            600GB per company database[^4]                              200GB                                            200GB                                                        100GB

## Agent Overview

AppGuard agents enforce AppGuard policy specified in the AGMS, which
involves periodically querying PDPs (Policy Distribution Points) for new
policies. As the agents note endpoint conditions and take actions, they
report on these by generating, signing, and encrypting log files that
are then sent to the LRPs (Log Retrieval Points) and from there back to
the AGMS.

The agent operates on desktops, laptops, tablets, VDIs, ATMs, and point
of sale devices. AppGuard Enterprise agents can also be deployed to a
non-persistent, virtual, desktop infrastructure (NP-VDI) such as Citrix
XenDesktop or Amazon Workspaces. Because pooled non-persistent desktops
have an identical system image, unique identifiers are created at IPL
time to ensure that these machines each get unique log files that don't
conflict with one another.

## Agent Requirements

Each Windows endpoint that will run the AppGuard agent requires as a
prerequisite installation of Microsoft KB4474419 or KB3033929.

For AGMS v6.5.x, agent version 6.5.x is compatible with the following
operating systems:

-   Windows 7 SP1+ (1) KB3035131 (or KB3153171) +(2) KB3033929 (KB
    application order is 1 and then 2), 8, 8.1, and 10 desktop systems.

-   Windows Servers v2008 R2 SP1 (Requires KB3033929), 2012 R2 2016,
    and 2019.

-   RedHat: 7.4, 7.5, 7.6, 7.7, and 8.1.

-   CentOS: 7.4, 7.5, 7.6, 7.7, and 8.1.

-   AWS 2 (with kernel version 4.14+).

    1.  ## Supported PDPs/LRPs

These FTP servers (and their operating systems) have been tested to work
with the AGMS:

-   IIS 6/Windows Server 2003, IIS 7.5/Windows 2008 R2, and IIS
    8.5/Windows 2012 R2, and IIS 10.0/2016.

-   vsftpd (all versions: https://security.appspot.com/vsftpd.html).
    Tested on FreeBSD and Linux.

-   Native BSD FTPD. Tested on OpenBSD. (Does not have SSL support for
    FTPS.)

-   Linux ProFTPd.

## Defining Sites and Groups

AppGuard implements protection, audits, and controls on devices located
anywhere using policies. The purpose of the site/Group-based policy
concept is to minimize administrative burden by defining configuration
settings for sites and policy settings for groups, rather than
individual policies for each end user in the population. Administrators
should group devices together that are expected to have a common set of
policies. These groups are not related to Active Directory groups. A
group may only have one role (i.e. a Workstation Role vs. a Server
Role).

Defining groups is the first step to implementing a successful AppGuard
deployment. What follows is a list of considerations that may influence
how groups are defined. Groups can always be re-defined after
deployment, and devices can always be moved from one group to another
within the same site.

+----------------------------------+----------------------------------+
| Consideration                    | Suggestion                       |
+==================================+==================================+
| Roles of the endpoints           | A separate policy group is       |
|                                  | required for each role. For      |
|                                  | instance, there could be a group |
|                                  | for Publishing Workstation vs.   |
|                                  | Development Workstation vs Web   |
|                                  | or File Servers.                 |
+----------------------------------+----------------------------------+
| Does the group consist of end    | Separating policy groups would   |
| users such as developers who     | allow such discretion to one     |
| must occasionally install tools  | group but deny it to another.    |
| or IT Administrators who can be  |                                  |
| trusted to suspend one or more   |                                  |
| anti-malware protections for     |                                  |
| legitimate, temporary needs?     |                                  |
+----------------------------------+----------------------------------+
| Are large sets of computers      | Create Custom Role Policies      |
| running sets of applications     | based on AppGuard policies.      |
| compared to other sets?          | Roles will apply to the largest  |
|                                  | sets of systems. Only add rules  |
|                                  | to Roles required to adjust      |
|                                  | operation for these broad sets   |
|                                  | of machines. Similarly, groups   |
|                                  | should likewise contain minimum  |
|                                  | rules needed to adjust operation |
|                                  | of applications that are         |
|                                  | included in all that group's end |
|                                  | points.                          |
+----------------------------------+----------------------------------+
| Are some computers mobile and    | Mobile assets often require      |
| others fixed?                    | different protections than       |
|                                  | desktops                         |
+----------------------------------+----------------------------------+
| Are some end user devices more   | More aggressive desktop          |
| exposed to risks than others?    | hardening policies may be        |
|                                  | appropriate for them, but not    |
|                                  | others.                          |
+----------------------------------+----------------------------------+
| What types of confidential       | For any group, a trusted enclave |
| information is stored on this    | could be defined to limit access |
| group's computers?               | of sensitive data to a subset of |
|                                  | applications.                    |
+----------------------------------+----------------------------------+
| Do employees bring their own     | Consider challenges related to   |
| equipment to work?               | employees needing to install and |
|                                  | maintain their own software. A   |
|                                  | separate group with relaxed      |
|                                  | suspend and uninstall rules      |
|                                  | could facilitate these users.    |
|                                  | Policies may need adjustment     |
|                                  | more frequently for these        |
|                                  | groups.                          |
+----------------------------------+----------------------------------+
| Do any end users possess         | An end user with local           |
| administrative privileges for    | administrator privileges cannot  |
| their computers? How are those   | override AppGuard policies,      |
| used?                            | including software installation, |
|                                  | without permission. A separate   |
|                                  | group, such as IT, can be        |
|                                  | created with more relaxed        |
|                                  | policies.                        |
+----------------------------------+----------------------------------+
| Are there any groups that        | Roll out to employees/groups     |
| include employees or executives  | with more flexibility first      |
| that have very high demands?     |                                  |
+----------------------------------+----------------------------------+
| Are there two or more different  | Role-based administration on the |
| IT entities that administer the  | AppGuard Management System can   |
| end user computers?              | restrict what data and controls  |
|                                  | IT personnel may view or alter.  |
+----------------------------------+----------------------------------+
| Are any end user software        | These users may require          |
| developers or testers?           | substantially different          |
|                                  | policies, such as the ability to |
|                                  | launch arbitrary executables     |
|                                  | from a specific space. Unified   |
|                                  | development/testing environments |
|                                  | will help limit policy changes.  |
+----------------------------------+----------------------------------+
| Are different computer           | Operational workflow             |
| configuration management systems | considerations may warrant       |
| used for different end users?    | defining separate groups.        |
+----------------------------------+----------------------------------+
| Is other client security         | Different vendor software of the |
| software in use by different     | same type (e.g., Anti-virus) do  |
| groups of end users?             | not require separate groups.     |
|                                  |                                  |
|                                  | However, if some computers are   |
|                                  | required to have different types |
|                                  | (disk encryption, etc.) of       |
|                                  | security software, separate      |
|                                  | group definitions may be         |
|                                  | appropriate.                     |
+----------------------------------+----------------------------------+
| Location of PDPs and LRPs        | When deploying on a global       |
|                                  | basis, PDPs and LRPs may be      |
|                                  | deployed in different parts of   |
|                                  | the world. If this is the case,  |
|                                  | consider multiple sites.         |
+----------------------------------+----------------------------------+

[^1]: If the AGMS is not installed on the same server as SQL Server, SQL
    Server Command Line Query Utility must be installed on the AGMS
    server. Additionally, depending on which version of SQL Server you
    use, you may have to download SQL Server Command Line Query Utility
    separately from Microsoft's website.

[^2]: Generally appropriate for 10,000 -- 50,000 endpoints.

[^3]: Generally appropriate for 50,000 -- 100,000 endpoints.

[^4]: Each agent may create up to 3.5Mb of data per day. The AGMS purges
    records every 30 days by default. Given these parameters, there is
    slightly more than enough storage space for 5000 agents' worth of
    log data.
