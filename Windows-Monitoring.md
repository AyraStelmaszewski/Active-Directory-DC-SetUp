The purpose of this readme is to reference fews windows monitoring tools with explaination. 

# Sysmon <br>

================================================================================================= <br>
*sources* <br>
https://syedhasan010.medium.com/sysmon-how-to-setup-configure-and-analyze-the-system-monitors-events-930e9add78d <br>
================================================================================================= <br>
Due to active development of the project, newer artifacts and evidence sources are constantly being added to Sysmon’s capabilities. 
However, you can get a quick idea on how Sysmon can aid you in identifying anomalous activities by checking this short list of features:
<br>

- Log process creation — command line, basic process information, parent process information, etc.
- Network communications — connection’s source processes, IPs, ports, hostnames, etc.
- Hashing process images — a hash of process image files is available in SHA1, MD5, SHA-256, or the IMPHASH format
- DLL loading — identify the loading of DLLs along with their hashes
- File modification — Identify changes in file creation times, a technique most commonly utilized by attackers to avoid detection
- Kernel-mode malware, rule filtering based on False Positives, session identification, correlations, and much more!

### Important points 

- Sysmon work in pair with a configuration file. If not created, there is the default one used.
- Network are not monitored by default
- Process image are hashed in SHA-1

### Find logs using GUI

==>  eventvwr.msc ==> Applications and Services ==> Microsoft ==> Windows ==> Sysmon ==> Operational 

### Create config file 

Since we haven't create a config file during our installation we'll do it now. <br>

- First we need to know our sysmon version
```powershell
Sysmon64.exe -? config
```
- Configuration file are .xml so let's create one.
- Structure of config file should be written in that way :
```powershell
<Sysmon schemaversion="4.90">
   ...
</Sysmon>
```
- Then we should specify our hash algorithms
```powershell
<Sysmon schemaversion="4.90">
   <HashAlgorithms>md5,sha256,IMPHASH</HashAlgorithms>
</Sysmon>
```
