## ****Building a Private IPFS Network.****

There are two types of IPFS networks: public and private. All files in the public IPFS network are accessible to everyone. Since most business applications, especially enterprise solutions, require full control over their data, making their networks publicly available is not an option. This is where IPFS privacy features could help close the network for certain entities.

In this IPFS tutorial, we will go through the process of creating a private IPFS network.

**IPFS:** A protocol and network designed to create a content-addressable, peer-to-peer method of storing and sharing hypermedia in a distributed file system. [Read more](https://ipfs.io/)

**Private IPFS:** Allows IPFS to only connect to other peers who have a shared secret key. With **IPFS** **private networks**, each node specifies which other nodes it will connect to. Nodes in that network don’t respond to communications from nodes outside that network. [Read more](https://github.com/ipfs/go-ipfs/blob/master/docs/experimental-features.md#private-networks)

### **A step-by-step IPFS tutorial for creating a private network**

By default, IPFS use the following ports:

**IPFS**

4001 – Communication with other nodes

5001 – API server

8080 – Gateway server

1. First we have to create 3 virtual machines, so for this we can use 3 different machines or DIgitalocean.
2. In my case I am using Digitalocean, so for this go to [https://www.digitalocean.com/](https://www.digitalocean.com/) and register your self. 
    
    ![Untitled](https://user-images.githubusercontent.com/53792139/199660787-3cfd70b6-d320-4498-acd3-ce35c2500ca6.png)
    
3. Once registered new project will be created automatically.
    
    ![Untitled](https://user-images.githubusercontent.com/53792139/199660990-a4ab4db5-3a6a-42fd-933a-68ca6981f3d5.png)
    
4. Now for creating virtual machines we have to click on create and select droplet.
    
    ![Untitled](https://user-images.githubusercontent.com/53792139/199661087-bbe2a3b3-52e1-4d8f-9d45-675a4958cf2c.png)
    
5. In this tutorial we are using Ubuntu machine with version 22.10 x64.
    
    ![Untitled](https://user-images.githubusercontent.com/53792139/199661136-9f360752-84ee-4e78-a466-21698e83a319.png)
    
6. Choose type and CPU as per need.
    
    ![Untitled](https://user-images.githubusercontent.com/53792139/199661216-1509135b-512d-4053-b95a-629594ccd3ed.png)
    
7. For authentication click on password and create a new password.
    
    ![Untitled](https://user-images.githubusercontent.com/53792139/199661277-50e09bb5-0c3e-4b5f-873d-cddca0399fd5.png)
    
8. Now we need three nodes node-00 which will be our bootstrap node, node-01 and node-02. So click on + icon to increase the number of droplets and name it accordingly as per your preference.
    
    ![Untitled](https://user-images.githubusercontent.com/53792139/199661321-0aba4068-3f5c-4372-8493-7524d3cee0c3.png)
    
9. Click on create droplet.
    
    ![Untitled](https://user-images.githubusercontent.com/53792139/199661404-c2e988e5-cea4-40d1-b049-3babf69f75cc.png)
    
10. All the droplet will be created.
    
    ![Untitled](https://user-images.githubusercontent.com/53792139/199661452-213012a8-c19a-4f2e-8416-9d698517f02b.png)
    
11. Now we have to access VM via CMD (command line) so for this, click on node-00 and statistics about node 00 will be displayed, make sure node-00 is turn on and it’s running if in your case it is off then toggle the button to turn on. Now click on console.
    
    ![Untitled](https://user-images.githubusercontent.com/53792139/199661546-b792b0d8-a0d7-46ce-8f2f-5e40444da03a.png)
    
12. In the same way open console for all the nodes.
    
    ![Untitled](https://user-images.githubusercontent.com/53792139/199661643-0f2ce904-3543-41aa-92ca-2a8f7d5881f9.png)
    
13. Now perform the following steps.
14. Download the Linux binary from **(https://dist.ipfs.tech/#kubo)**.
    
    `wget https://dist.ipfs.tech/kubo/v0.16.0/kubo_v0.16.0_linux-amd64.tar.gz`
    
15. Unzip the file:
    
    `tar -xvzf kubo_v0.16.0_linux-amd64.tar.gz`
    
16. Move into the `kubo` folder and run the install script:
    
    `cd kubo`

    `sudo bash install.sh`
    
17. Test that IPFS has installed correctly:
    
    `ipfs --version`
    
18. Now type: 
    
    `nano /usr/lib/systemd/system/ipfsd.service`
19. Now paste following code inside ipfsd.service:
    
    ```java
    [Unit]
    Description=ipfs daemon
    
    [Service]
    ExecStart=/usr/local/bin/ipfs daemon
    Restart=always
    User=root
    Group=root
    
    [Install]
    WantedBy=multi-user.target
    ```
    
20. Now press `ctrl+x` to exit 
21. Once done initialize IPFS in all node by: 
    
    `ipfs init`
22. Remove bootstrap from all nodes: 
    
    `ipfs bootstrap rm -all`
23. Now change IPFS configuration as follows:
    
    ```java
    ipfs config Addresses.Gateway /ip4/0.0.0.0/tcp/8080
    ipfs config Addresses.API /ip4/0.0.0.0/tcp/5001
    ipfs config show
    ```
    
24. Force to private network: 
    
    `export LIBP2P_FORCE_PNET=1`
25. Restart IPFS daemon: 
    
    `systemctl restart ipfsd`
26. Check status: 
    
    `systemctl status ipfsd`
27. Perform step 14 to 26 in all three node.
28. Done, now check id of bootstrap node (node-00): 
    
    `ipfs id`
29. Copy id of path of bootstrap node (node-00) followed by IP address. In my case id is,
    
    `/ip4/164.92.65.166/udp/4001/quic/p2p/12D3KooWCkbu1irMu5651hvDEMQuwK6kLfiJCqpRTnD2az7DjtSz`
    
30. Now execute below command in other nodes: 
    
    `ipfs bootstrap add /ip4/ipaddress/udp/4001/quic/p2p/peerid`
    
31. Now run following script in bootstrap node (node-00) to see list of all peers: 
    
    `ipfs swarm peers`
32. Now our IPFS private network has created, for test let’s add a txt file from node-01 and will try to access it inside node-02 and node-00.
33. For creating file run following script in node-01: 
    
    `echo "IPFS P2P NETWORL (node-01)" > info.txt`
    
    ![Untitled](https://user-images.githubusercontent.com/53792139/199662597-9e2eea02-2869-4c27-a853-f9ebc121dfca.png)
    
34. Add info.txt in IPFS network by using following command: 
    
    `ipfs add info.txt`
    
    ![Untitled](https://user-images.githubusercontent.com/53792139/199662678-dd1a93b1-20ee-45ba-97df-694a05bbe862.png)
    
35. Copy the generated hash for accessing file in node-02 uploaded from node-01.
    
    ![Untitled](https://user-images.githubusercontent.com/53792139/199662789-f81541eb-adf4-4a80-b507-7620e555cfff.png)
    
36. Now in node-02 run the following command:
    
    `ipfs cat QmeHE2NBWN4N33uW1Ncsc5TDqy4kuK1YuEwMpXcZq37SHr`
    
    ![Untitled](https://user-images.githubusercontent.com/53792139/199662855-bc4e1353-1adb-4ba6-8a6a-93203b2203b3.png)
    

36. We can successfully see the contents of file which was uploaded from node-01.

37. Same way can see the content in node-00 also.

    ![Untitled](https://user-images.githubusercontent.com/53792139/199662955-0b2160dd-ee44-4a46-b5b0-3e8233411daa.png)
