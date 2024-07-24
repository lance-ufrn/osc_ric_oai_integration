# OSC RIC and OpenAirInterface Integration
This tutorial aims to integrate the OpenAirInterface (OAI) and the O-RAN Software Community RIC (OSC RIC). It presents step-by-step instructions for installing and deploying OAI Core 5G, OAI gNodeB in CU/DU split mode and OSC RIC in Release J.

* Installation environment:
  * Virtual machine (VirtualBox)
  * Ubuntu 20.04.6 LTS
  * RAM: 16 GB
  * 4 CPUs


## FlexRIC Installation
FlexRIC is the OAI solution for the RAN Intelligent Controller (RIC). Although OSC RIC is used in this tutorial, in order to integrate OAI with OSC RIC it is necessary to install FlexRIC, as it installs the Service Models used by OAI gNodeB.

* Update and Upgrade:
```sh
sudo apt-get update
sudo apt-get upgrade
```

* Install dependencies:
```sh
sudo apt-get install -y python3.8 git cmake-curses-gui autotools-dev automake g++ make libpcre2-dev byacc cmake python3-dev libsctp-dev pcre2-utils bison
```

* Install SWIG:
```sh
git clone https://github.com/swig/swig.git
cd swig
git checkout release-4.1
./autogen.sh
./configure --prefix=/usr/
make -j8
sudo make install
```

* Install FlexRIC:
```sh
git clone https://gitlab.eurecom.fr/mosaic5g/flexric flexric
git checkout beabdd072ca9e381d4d27c9fbc6bb19382817489
mkdir build && cd build && cmake .. && make -j8
sudo make install
```

## OpenAirInterface Installation
In this section, we will install the OAI in the simulated version and with the E2 interface enabled.
```sh
git clone https://gitlab.eurecom.fr/oai/openairinterface5g oai
git checkout e8a083767af14d7b36c0624f3dc586aae07711bc
cd cmake_targets/
./build_oai -I -w SIMU --gNB --nrUE --build-e2 --ninja
```

## OSC RIC Release J installation
In this section, we will install the OSC RIC from the official O-RAN repository, using release J.

* Enter in sudo mode and update:
```sh
sudo -i 
apt update -y
apt upgrade -y
```

* Install dependencies:
```sh
apt install -y build-essential python3-pip curl tree net-tools build-essential nfs-common openssh-server lksctp-tools autoconf libtool flex libboost-all-dev apt-utils
```

* Clone the official O-RAN repository:
```sh
git clone "https://gerrit.o-ran-sc.org/r/ric-plt/ric-dep" -b j-release
```

* Go into the installed repository and then into the bin folder:
```sh
cd ric-dep/bin
```

* Install kubernetes, kubernetes-CNI, helm and docker:
```sh
./install_k8s_and_helm.sh
```

**IMPORTANT**: Before next steps, in the file *../RECIPE_EXAMPLE/example_recipe_oran_j_release.yaml*, change the variables `ricip` and auxip from the default value 10.0.0.1 to the IP address of the virtual machine


* Install chartmuseum into helm and add ric-common templates:
```sh

```