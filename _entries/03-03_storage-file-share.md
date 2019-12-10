---
sectionid: fileshare
sectionclass: h2
title: File Sharing (SMB/CIFS)
parent-id: storage
---

### Network (Shared) Volumes

Upon occasion, a Windows system may still mount and use drives mounted via SMB. These can be attached (mapped) to drive letters, although it's generally not necessary. It is simple to copy a remote file directly, e.g., `copy \\host\sharevol\path1\..\pathn\file.txt \localpath\file.txt`.

Universal naming convention (UNC) paths usually take the form `\\host\sharevol` or `file://host/sharevol/`. In some cases, especially SAMBA clients and servers, UNCs may appear with forward slashes: `//host/sharevol`.

> Older scripts may show other formulations for UNC's, e.g., `file://\\host\sharevol`. Later versions of Windows are less particular about UNC syntax.

Volumes may be mounted at the share level (i.e., `file://host/sharevol`) or "deep-linked" `file://host/directory`.

Permissions are checked on the underlying resources and if an SMB session between the client and the server doesn't exist, the client will be authenticated.


### SMB Sessions

Use of Server Message Block (SMB) (sometimes known as Common Internet File Sharing, or CIFS) is ubiquitous in Windows. SYSVOL replication, DNS zone replication and many other infrastructure roles use SMB under the covers to exchange data.

#### SMB on the Network

SMB dates back to the 1980's, but was rearchitected (since Windows 2000) to operate on TCP/IP (Direct-host SMB) over port 445/tcp/udp. No other ports are required for SMB itself, although some Windows processes may require other common ports, like Kerberos, LDAP, SSL/TLS, and the RPC endpoint mapper (135/tcp).

> NetBIOS 
> Don't confuse NetBIOS with SMB, RPC, or any other legacy networking interface.
> NetBIOS is a legacy (largely extinct) interface for network applications. It was largely removed as a OS dependency in Windows Server 2000, although applications continued to depend on it throughout the early 2000's. WINS was a distributed network service to allow NetBIOS name announcements to be broadcast across TCP subnets.
> NetBIOS over TCP/IP (NBT) ports should never be opened to enable file service traffic, and NBT should be disabled on all systems by default.
> WINS should be disabled and never used in any cloud networking context.

#### SMB Authentication

SMB is session-oriented, which means that every unique combination of host/user security context is individually authenticated, and a durable access token is created and cached by the client in the context of the user. This happens at the time of first connection.

Services which make SMB connections will do so in the context of the assigned service account.

#### Command line usage

NET.EXE is the command line tool to manage network connections at the Windows CMD prompt. The USE subcommand allows you to connect and disconnect resources.

Connect to a file share

`NET USE [drive_letter] file://myserver/myshare /user:mydomain\myaccount *`  (where * stipulates that you will be prompted for your password). The drive_letter is optional; you will establish a connection with or without it, but the drive mapping gives you a shorthand way to refer to the remote file path.

See all connected (mapped) drives using the NET USE command. Note that it is possible to do loopback and connect to a local shared resource via SMB:

> An image will go here

Use `NET USE [drive_letter] /D` to disconnect a specific connection, or `NET USE * /D` to disconnect everything.


#### Direct object reference

UNC paths, like URLs, can directly reference an object, e.g.,

As mentioned above, `file://myserver/myshare/path-1/.../path-n/mydocument.doc`. This can be useful in scripting when only a specific object is needed. However, it is important to note that a durable SMB session context will still be created.

#### Deep linking

`NET USE [drive_letter] file://myserver/myshare/path-1/.../path-n` allows you to alias a path within a share.


#### Implicit (default) shares

Some system items are shared automatically, e.g., disk volumes. To connect to a system's C drive, you can

`NET USE [drive_letter] file://myclient/c$`. You must provide adequate permissions to connect; the system drive requires local system admin privileges. Other implicit shares include:

`IPC$` - interprocess communication object

`ADMIN$` - `%windir%` (typically `C:\WINDOWS`)

`{DRIVE}$` - each drive letter with a volume attached

See all shared local drives using the NET SHARE command:

> An image will go here

### SMB Authentication

A single SMB session is all that is needed to support many individual connections, so you only need to specify user credentials on the first connection to a host.

If you want to have an authenticated SMB session to a host for any subsequent connections, you can:

`NET USE file://myserver/IPC$/user:mydomain\myaccount *`

`NET USE file://myserver/ADMIN$` creates a connection to the system folder and requires admin credentials on the remote system.

> Clients are servers too!
> Unless specifically disabled, every Windows system can both use and share content.

### SMB Special Volumes

Windows Explorer or the "net view" command at the command line can be used to enumerate a remote host's shares, e.g., `NET VIEW file://remhost`.

Some shares on a host are hidden:

- `C$`, `D$`, etc. are hidden (implicit) shares that allow direct shared access to the root of a remote file system.
- `ADMIN$` refers to the remote hosts %systemroot%, e.g., c:\windows.

`IPC$` is a special share which allows an authenticated session on the host without mapping any file system resources. It isn't typically used interactively, although it can be.

