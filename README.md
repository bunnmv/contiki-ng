<img src="https://github.com/contiki-ng/contiki-ng.github.io/blob/master/images/logo/Contiki_logo_2RGB.png" alt="Logo" width="256">

# Contiki-NG: The OS for Next Generation IoT Devices

[![Build Status](https://travis-ci.org/contiki-ng/contiki-ng.svg?branch=master)](https://travis-ci.org/contiki-ng/contiki-ng/branches)
[![Documentation Status](https://readthedocs.org/projects/contiki-ng/badge/?version=master)](https://contiki-ng.readthedocs.io/en/master/?badge=master)
[![license](https://img.shields.io/badge/license-3--clause%20bsd-brightgreen.svg)](https://github.com/contiki-ng/contiki-ng/blob/master/LICENSE.md)
[![Latest release](https://img.shields.io/github/release/contiki-ng/contiki-ng.svg)](https://github.com/contiki-ng/contiki-ng/releases/latest)
[![GitHub Release Date](https://img.shields.io/github/release-date/contiki-ng/contiki-ng.svg)](https://github.com/contiki-ng/contiki-ng/releases/latest)
[![Last commit](https://img.shields.io/github/last-commit/contiki-ng/contiki-ng.svg)](https://github.com/contiki-ng/contiki-ng/commit/HEAD)

Contiki-NG is an open-source, cross-platform operating system for Next-Generation IoT devices. It focuses on dependable (secure and reliable) low-power communication and standard protocols, such as IPv6/6LoWPAN, 6TiSCH, RPL, and CoAP. Contiki-NG comes with extensive documentation, tutorials, a roadmap, release cycle, and well-defined development flow for smooth integration of community contributions.

Unless explicitly stated otherwise, Contiki-NG sources are distributed under
the terms of the [3-clause BSD license](LICENSE.md). This license gives
everyone the right to use and distribute the code, either in binary or
source code format, as long as the copyright license is retained in
the source code.

Contiki-NG started as a fork of the Contiki OS and retains some of its original features.

Find out more:

* GitHub repository: https://github.com/contiki-ng/contiki-ng
* Documentation: https://github.com/contiki-ng/contiki-ng/wiki
* Web site: http://contiki-ng.org
* Nightly testbed runs: https://contiki-ng.github.io/testbed

Engage with the community:

* Gitter: https://gitter.im/contiki-ng
* Twitter: https://twitter.com/contiki_ng


# Marcus' notes and Contiki-NG cheat sheet 

Below are listed a series of useful commands and other tips for Contiki NG and Cooja usage focused on MacOS and Docker environment.

1. MacOS adjustments for Docker usage

   * Include in ~/.bash_profile:
    ```bash
    export CNG_PATH=<absolute-path-to-your-contiki-ng>
    alias contiker="docker run --privileged \
    --mount type=bind,source=$CNG_PATH,destination=/home/user/contiki-ng \
    -e DISPLAY=docker.for.mac.host.internal:0 \
    -ti contiker/contiki-ng"
    ```
2. MacOS adjustments XQuartz

   * Use this version of socat command for X11 server
    ```bash
    socat TCP-LISTEN:6000,reuseaddr,fork UNIX-CLIENT:\"$DISPLAY\" &
    ```
    * In case the port is in use kill the process and try again (if possible, if not the port has to be changed in the Docker)
    ```bash
    lsof -nP -iTCP:6000 | grep LISTEN
    kill -9 $PID
    ```
3. Run Contiker Docker

   * Start **new container** and run bash
    ```bash
    contiker bash
    ```
* Start **new container** and run Cooja

    ```bash
    contiker cooja
    ```  
4. Usefull Docker commands

   * List all Docker images in your host
    ```bash
    docker image ls -a
    ```
   * List all Docker Containers ( Containers are instances of images, you can have N containers for 1 img)
    ```bash
    docker container ls -a
    ```
    * Rename container
    ```bash
    docker container <CONTAINER-ID | CONTAINER-NAME> rename <NEW-NAME>
    ```
    * Start container
    ```bash
    docker container <CONTAINER-ID | CONTAINER-NAME> start
    ```
     * Stop container
    ```bash
    docker container <CONTAINER-ID | CONTAINER-NAME> stop
    ```
5. Run cooja on docker from its bash

    * Using shortcut in home dir:
    ```bash
    ~/cooja
    ```  
    
    * Or going to Cooja dir in contiki-ng/tools/cooja and running:
    ```bash
    // run cooja
    ~/contiki-ng/tools/cooja$ ant 
    
    // run cooja with higher Heap memory available to Java. 
    // Usefull for big simulations 1536M instead of 512M
    ~/contiki-ng/tools/cooja$ ant run_bigmem 
    
    // run without GUI 
    // ../../.. relates to home dir
    // all paths are realtive paths
    
    ~/contiki-ng/tools/cooja$ ant run_nogui -Dargs=../../../examples/benchmarks/rpl-req-resp-modified/sim_100.csc
    ``` 
    
6. Advanced Cooja Run: No GUI + increased heap mem 

    * Run cooja without GUI in **Cooja DIR**:
    ```bash
    ~/contiki-ng/tools/cooja$ java -mx1536m -jar dist/cooja.jar -nogui="../../examples/benchmarks/rpl-req-resp-modified/sim_100.csc" -contiki="../.."
    ```      
7. Run command on different folder without leaving the folder

    ```bash
    ~/contiki-ng/tools/cooja$ (cd ~/contiki-ng/examples/benchmarks/rpl-req-resp-simple/ && make TARGET=cooja clean)

    ```
