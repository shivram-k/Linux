# Apt Update Hash Sum mismatch.Some index files failed to download. They have been ignored, or old ones used instead.

  In Ubuntu's this above common error's occurs mostly while upating the OS.The fix has been added below. 

## Fix:1

     sudo apt-get clean 
     sudo rm -rf /var/lib/apt/lists/*  
     sudo apt-get upgrade -y 
     sudo apt-get update 

## Fix:2

     sudo apt-get clean 
     sudo rm -rf /var/lib/apt/lists/*
     sudo apt-get update -o Acquire::CompressionTypes::Order::=gz
     sudo apt-get upgrade -y
     sudo apt-get update

## Fix:3



Run apt-get update or update and find out what key words to search for in /etc/apt:
        
            sudo apt-get -y update

        Err:24 http://in.archive.ubuntu.com/ubuntu focal/main DEP-11 64x64 Icons
        Hash Sum mismatch
        Hashes of expected file:

        Filesize:162899 [weak]
        SHA256:9f4ffba2a05dc2fb76857d4b30ed55724fe6e2999341694f2b86940ab067a44b
        SHA1:f470f9b518e5cd213a66334f3a5e3b671c4dbe61 [weak]
        MD5Sum:504d24e4682a64a0559d13f7cf1e5ca6 [weak] Hashes of received file:
        SHA256:f075b7d5d88090d77b8ff5e2cf9fe5dfbee51241ad105d3a2586dbc64ce80d81 
        SHA1:3e4f6efa90fd7ae3b6a2a35d3a95433d8bb582c8 [weak]
        MD5Sum:f7120bf35f298f4adfe070a329e1da33 [weak]
        Filesize:162899 [weak] Last modification reported: Mon, 20 Apr 2020 09:35:48 +0000 Release file created at: Thu, 23 Apr 2020 17:33:17 +0000
        
        ...
    
        
        
   In this case, the keyword is **"DEP-11"**
    
   Search the /etc/apt tree for the keyword(s):
      
     sudo find /etc/apt -type f -exec egrep -in "DEP-11" "{}" /dev/null ";" 
        
        /apt.conf.d/50appstream:1:## This file is provided by appstreamcli(1) to download DEP-11
        /apt.conf.d/50appstream:6: deb::DEP-11  {
        /apt.conf.d/50appstream:9: Description "$(RELEASE)/$(COMPONENT) $(NATIVE_ARCHITECTURE) DEP-11 Metadata";
        /apt.conf.d/50appstream:15: # Normal-sized icons for GUI components described in the DEP-11
        /apt.conf.d/50appstream:17: deb::DEP-11-icons {
        /apt.conf.d/50appstream:20: Description "$(RELEASE)/$(COMPONENT) DEP-11 64x64 Icons";
        /apt.conf.d/50appstream:27: # the DEP-11 YAML metadata.
        /apt.conf.d/50appstream:28: deb::DEP-11-icons-hidpi {
        /apt.conf.d/50appstream:31: Description "$(RELEASE)/$(COMPONENT) DEP-11 128x128 Icons";
    
    
   File with issues will be listed, move it somewhere just in case this doesn't work:

    ~   sudo mv -f /apt.conf.d/50appstream /tmp 

        sudo apt-get -y clean  
        sudo rm -rf /var/lib/apt/lists/*  
        sudo find /var/lib/apt -type d -name "partial" -exec rm -rf "{}" ";"  
        sudo apt-get upgrade -y 
        sudo apt-get update -y
