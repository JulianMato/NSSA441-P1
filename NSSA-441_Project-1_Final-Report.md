## Intro list
- [x] Intro
## Technology Coverage List    
- [x] CLOS & fat tree Topology
- [x] BGP & EBGP
- [x] Full Mesh & Partial Mesh    
## Experements List    
- [x] Topology discription & reason for use
- [] break link in Spine & ToR **convergence time**          
- [] round trip time inter & intra pod          
- [] Add new router to spine, leaf, and ToR             
- [] add new host or server    
## Results List
- [] break link in Spine & ToR **convergence time**          
- [] round trip time inter & intra pod          
- [] Add new router to spine, leaf, and ToR             
- [] add new host or server     
## Discussion of results list
- [] Discussion of results
## Critical Summary list
- [] Critical Summary                   

- [] Spell Check

BGP in the Datacenter
=====================             
## Table of Contents
- [Introduction](#introduction)
- [Technology Used](#technology-used)           
    - [Clos & Fat Tree](#clos--fat-tree) 
    - [BGP & EBGP](#bgp--ebgp)
    - [Full Mesh & Partial Mesh](#full-mesh--partial-mesh)
- [Experements](#experements)
    - [Topology Description and Benefits](#topology-description-and-benefits)
    - [Link Breakage from Spine to Leaf and from Leaf to ToR](#link-breakage-from-Spine-to-leaf-and-from-leaf-to-tor---experement)
    - [Inter & Intra Pod Travel Time](#inter--intra-pod-travel-time---experement)
    - [Expand by One Spine, Leaf, and ToR](#expand-by-one-spine-leaf-and-tor---experement)
    - [Deploy a New Server](#deploy-a-new-server---experement)
- [Results](#results)
    - [Link Breakage from Spine to Leaf and from Leaf to ToR](#link-breakage-from-Spine-to-leaf-and-from-leaf-to-tor---result)
    - [Inter & Intra Pod Travel Time](#inter--intra-pod-travel-time---result)
    - [Expand by One Spine, Leaf, and ToR](#expand-by-one-spine-leaf-and-tor---result)
    - [Deploy a New Server](#deploy-a-new-server---result)
- [Discussion of results](#discussion-of-results)
- [Critical Summary](#critical-summary)
- [References](#references)
- [Appendix](#appendix)
<br>
<br>
<br>

## Introduction:
This paper will talk about the use of BGP deployed in a Clos tapology for use in the datacenter. BGP or Border Gateway protocol is a widly used routing protocol due to its stability and its  "capability to support large and complex networks"(Zhang and Bartell, ch.1). To match the scalability of BGP an equaly scalable tapology must be used, this is where Clos steps in. Clos topologys are relativeley old being proposed more than 60 years ago and seeing extensive use in telephony and computing. In this paper we will be using a variant of Clos comonly refered to as "Fat Tree". Fat Tree takes advantage of Clos networks masive expandability but introduces a strict hierarchy which aids in its deployment and maintenence. In parallel the hierarchical form of Fat Tree aids in the desighn, implemintation, and oporation of BGP by giving it a highly structured network.            

We will begin by taking a more indepth look at Clos & Fat Tree, BGP & EBGP, along with Full Mesh & Partial Mesh. This will give you the base line knowladge needed to understand the Experemental portion of the paper. The experemental portion begins with a description of the topology used and a fiew key test and the prosedure used to run them, followed by the results section which will lay out the results as factualy and unbiased as posible. Folowing will be an interpritation of the data along with a critical look at the clames relative to the data.
<br>
<br>
<br>

## Technology Used:

### Clos & Fat Tree
Fat Tree(A subset of Clos topologys) relyes hevily on route agregation. more spesificaly as you see in figure 1 each level has a "thicker" connection than the last. Clos and there fore Fat Tree traditionaly use a 2 or 3 teired network structure with the "Spine" being at the top followed by "leafs" and in a 3 teired Clos a final ToR layer. The Spine acts as the main distrobution between otherwise isolated pods of systems. The leafs are the agrigation layer, they collect and truncate trafic from the ToR layer as to pass it through to the Spine. Leaf layers are also responsable for any IP Sec or other security mesures implemented by the system, this is crusial due to the need to keep the Spine as secure as posible. With the specialized nature of each layer relativeley specific hardware considerations have to be made, the Spine needs reletively fiew connections but needs massive bandwith to handle the agrigated leaf layer trafic, leaf layers need a large amount of ports to acomodate as many hosts or ToR's with a limited number of high speed links to the Spine, and finaly the ToR layer only needs a large number of low speed connections. By having all of your trafic agregate adding new portions to the network is trivial giving Clos a tremendous scalability while remaining structured and orginized.

![Figure 1](Fat_tree_network.png "Fat Tree")
<br>
<br>

### BGP & EBGP
BGP "is the routing protocol that glues together the net-works forming the Internet"(Garcia-Martinez and Bagnulo 2432). BGP works on the concept of neighbers, route advertiesments, hello timers, and route flooding. To be exact we are using EBGP to simplify our topology, in breif EBGP advertieses and re-advertises routes to all BGP enabled devices. A neighber in BGP is simply the next hop or directly adgacent BGP aware device that a BGP device has an established trust with, to establish a trust a BGP device must know the devices correct AS or Autonumus System number and the IP address of the connected link. This is how BGP maintains its isolation from other BGP or malitous networks, without intimate knowladge of the network not mouch can be done. Route advertiesments are as they sound, when a BGP device wants to share the location of a network directly atached to them they can addvertise it to the network thus propogating its location across all BGP routing tables. Hello timers are packets sent from neighber to neighber to simply afirm of its continued function in the network, normaly hello timers are sent at a predefined interval and if ever 3 accnoladgments are missed in a row the network is flooded with a route chainge. Finaly flooding is the act of propogating information to all systems on the network.
<br>
<br>

### Full Mesh & Partial Mesh
Full Mesh is a term to reffer to the total interconection of all nodes in a network as shown in figure 2. This allows for maximum pioints of redundency incase of a link falure, for this reason all the pods in a Clos or Fat Tree topology are interconnected. This is best practice in most networks, though it does bring with it some issues namley loops. A large issue with full mesh is the existence of loops in the network leaving the opertunity for packets to get lost in limbo slowing down the network and eventualy crashing the machines tunning it. As to avoid this we have loop avoidence protocols but by adding an other protocol to your network you are adding latency, and unfortionatly the more links the longer convergence could take as the protocol searches for a new route. this can be mitigated by adding link state costs to narow the desision tree but if done correctly in a 3 teir Clos topology the redundency allready present at the spine a partial mesh can be used while maintainging redundency. Partial Mesh as it sounds only connects a portion of the systems together leaving a higher chance of falure unless acounted for though it does aleviate network complexity and convergence time at times.

![Figure 2](NetworkTopology-FullyConnected.png "Full Mesh")
<br>
<br>
<br>

## Experements:

### Topology Description and Benefits
With our implimentation of Clos & Fat Tree we used 4 Spine routers, an other 4 leaf routers 2 for each Pod, 2 racks with 1 ToR router each, and one host per rack. This configuration was chosen due to it being the minimum hardware to showcase and run BGP while retaining a highe level of expandability if needed. The IP and AS naming scheme is standardised across the layers starting with the first octet denoting what layer the port is going to, anything with a one is going to the spine and a two is connected to the leaf. Octet two denotes if there is a segmentation souch as the pods in the leaf and ToR layer's. Octet 3 denotes a second reddundent connection if any. And the fourth and last octet denotes the /30 link between two BGP neighbers, /30 was used in this instince due to it only having two host ip addresses making it unfeasable for someone to acess the same network as the two neighbers. Along with a higherarcical ip scheme the AS's are also structured. All Spine AS's are a multiple of 25 starting at 0 as to keep the first digit unchainged to denote its layer from 1-3, 1 being the spine and 3 being the ToR.             

The host names are also telling of position and location in the network, *N* denotes the network in this case there is only one, *S* denotes the spine and its number in the spine, *P* is the pod number, *L* designates the leafs and there corosponding number, and finaly *ToR* shows what rack that machine is in. By having souch a structured network we can insure that reguardless of scale the network can remain human readable incase of a future expansion or in the case of maintenence. Finaly where it was posible The port numbers for connecting machines match to make diagnostics easyer.

Some benefits to this topology are that in the case of any one link falure reguardless of placment the topology will retain full connectivity once converged, and with key exeptions the network could withstand two to three link falures with no loss of connectivity. BGP also alows for previously down links with a higher scores than those curently in use to re-establish themselves as the primary link. BGP's features and this topologys desighn esentialy ensures at leas one hot standby at all times during peik oporation. All in all BGP's route awarness and Clos's expandable and redundant nature create a highly flexable and relyable system.
<br>
<br>

#### *Spine*
| Host Name | Interface 1(E1/1) | Mask | Interface 2(E1/2) | Mask | AS | Advertised Network's |
| :--- | ---: | :--- | ---: | :--- | :---: | :--- |
| N-1_S-1 | 1.1.1.1 | /30 | 1.1.1.5 | /30 | 124 | N/A |
| N-1_S-2 | 1.1.1.13 | /30 | 1.1.1.17 | /30 | 149 | N/A |
| N-1_S-3 | 1.1.1.25 | /30 | 1.1.1.29 | /30 | 174 | N/A |
| N-1_S-4 | 1.1.1.37 | /30 | 1.1.1.41 | /30 | 199 | N/A |
<br>

#### *Leaf*
| Host Name | Interface 1(E1/1) | Mask | Interface 2(E1/2) | Mask | Interface 3(E1/3) | Mask | Interface 4(E1/4) | Mask | AS | Advertised Network's |
| :--- | ---: | :--- | ---: | :--- | ---: | :--- | ---: | :--- | :---: | :--- |
| N-1_P-1_L-1 | 1.1.1.2 | /30 | 1.1.1.26 | /30 | 2.1.1.1 | /30 | 2.1.1.5 | /30 | 224 | N/A |
| N-1_P-2_L-2 | 1.1.1.14 | /30 | 1.1.1.38 | /30 | 2.1.1.9 | /30 | 2.1.1.13 | /30 | 249 | N/A |
| N-1_P-1_L-3 | 1.1.1.6 | /30 | 1.1.1.30 | /30 | 2.2.1.1 | /30 | 2.2.1.5 | /30 | 274 | N/A |
| N-1_P-2_L-4 | 1.1.1.18 | /30 | 1.1.1.42 | /30 | 2.2.1.13 | /30 | 2.2.1.9 | /30 | 299 | N/A |  
<br>

#### *ToR*
| Host Name | Interface 1(E1/1) | Mask | Interface 2(E1/2) | Mask | Interface 3(E1/3) | Mask | AS | Advertised Network's |
| :--- | ---: | :--- | ---: | :--- | ---: | :--- | :---: | :--- |
| N-1_P-1_ToR-1 | 2.1.1.2 | /30 | 2.1.1.10 | /30 | 192.1.1.254 | /24 | 324 | 192.1.1.0/24 |
| N-1_P-1_ToR-2 | 2.1.1.14 | /30 | 2.1.1.6 | /30 | 192.1.2.254 | /24 | 349 | 192.1.2.0/24 |
| N-1_P-1_ToR-3 | 2.2.1.2 | /30 | 2.2.1.10 | /30 | 192.2.1.254 | /24 | 374 | 192.2.1.0/24 |
| N-1_P-1_ToR-4 | 2.2.1.14 | /30 | 2.2.1.6 | /30 | 192.2.2.254 | /24 | 399 | 192.2.2.0/24 |             
<br>

![Figure 3](img-top-fing.png "topology")\
<br>
<br>

### Link Breakage from Spine to Leaf and from Leaf to ToR - Experement      
One of the main features of a meshed topology and BGP is to be able to re-establish connection after a broken link, so for this experement we will be testing that ability. we start by generating large ammounts of TCP and UDP trafic from K-Host-1 to K-Host-3 with the help of `nmap -p0-65535 192.2.1.1 -T5` and `nmap -sU -p- 192.2.1.1 -T5`. once the trafic starts to be generated we break a link. To begin with we broke 1.1.1.36/30 5 times followed by breaking link 2.1.1.8/30. K-Host-3 has a wireshark capture running on it, we log when the trafic stops and when it re-establashes connection and take the diffrence. Since the hello packet interval is set to 10 seconds and the LSA interval is 32 seconds we should be looking at an average of 31 seconds till convergance.
<br>
<br>

### Inter & Intra Pod Travel Time - Experement          
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Aenean sed adipiscing diam donec adipiscing tristique risus nec. Adipiscing elit duis tristique sollicitudin nibh sit amet commodo. Dignissim cras tincidunt lobortis feugiat vivamus at. Non blandit massa enim nec dui nunc mattis enim. Egestas egestas fringilla phasellus faucibus scelerisque eleifend donec pretium vulputate. Quis auctor elit sed vulputate mi sit. Euismod quis viverra nibh cras pulvinar mattis nunc. Tellus cras adipiscing enim eu turpis egestas. Pellentesque habitant morbi tristique senectus et. Id faucibus nisl tincidunt eget nullam non nisi est. Velit ut tortor pretium viverra suspendisse potenti nullam ac tortor. Tortor dignissim convallis aenean et tortor. Eu mi bibendum neque egestas congue quisque egestas diam. Varius duis at consectetur lorem donec massa. Scelerisque felis imperdiet proin fermentum leo. Ut diam quam nulla porttitor. In nulla posuere sollicitudin aliquam ultrices. Pulvinar sapien et ligula ullamcorper malesuada proin libero nunc consequat. Vel risus commodo viverra maecenas accumsan lacus vel facilisis.

Elit scelerisque mauris pellentesque pulvinar. Nulla malesuada pellentesque elit eget gravida cum sociis natoque penatibus. In ornare quam viverra orci sagittis eu volutpat odio facilisis. Lorem ipsum dolor sit amet. Libero nunc consequat interdum varius sit. Nunc mi ipsum faucibus vitae aliquet nec. Condimentum vitae sapien pellentesque habitant morbi tristique. Facilisi nullam vehicula ipsum a. Nec sagittis aliquam malesuada bibendum arcu. Integer enim neque volutpat ac. Vel turpis nunc eget lorem. Vitae congue eu consequat ac felis donec et odio.
<br>
<br>

### Expand by One Spine, Leaf, and ToR - Experement         
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut porttitor leo a diam sollicitudin tempor id eu nisl. Vulputate mi sit amet mauris. Nisi est sit amet facilisis magna etiam tempor orci eu. Non consectetur a erat nam at lectus urna duis convallis. Et molestie ac feugiat sed lectus vestibulum mattis. Neque viverra justo nec ultrices dui. Risus at ultrices mi tempus imperdiet nulla. Nunc consequat interdum varius sit amet. At augue eget arcu dictum varius duis.

Felis eget nunc lobortis mattis aliquam faucibus. Augue interdum velit euismod in. Massa massa ultricies mi quis hendrerit dolor magna eget est. Orci eu lobortis elementum nibh. Sed egestas egestas fringilla phasellus faucibus scelerisque eleifend donec. Facilisi morbi tempus iaculis urna id volutpat lacus. Platea dictumst vestibulum rhoncus est. Aliquam etiam erat velit scelerisque in. Lacus suspendisse faucibus interdum posuere. Dui sapien eget mi proin sed libero enim sed faucibus.
<br>
<br>

### Deploy a New Server - Experement            
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Magna etiam tempor orci eu lobortis. Vel elit scelerisque mauris pellentesque pulvinar pellentesque habitant morbi tristique. Blandit turpis cursus in hac habitasse platea dictumst quisque sagittis. Odio morbi quis commodo odio aenean sed adipiscing. Pellentesque habitant morbi tristique senectus. Lacus sed turpis tincidunt id aliquet risus feugiat in. Aliquet bibendum enim facilisis gravida. Ante metus dictum at tempor commodo ullamcorper. Ultrices neque ornare aenean euismod elementum nisi quis. Tortor dignissim convallis aenean et tortor at risus. Purus ut faucibus pulvinar elementum integer enim neque volutpat ac. Luctus venenatis lectus magna fringilla urna porttitor rhoncus. Mattis enim ut tellus elementum sagittis vitae et leo. Pharetra pharetra massa massa ultricies mi quis hendrerit dolor magna.

Ut sem viverra aliquet eget sit amet. Ac odio tempor orci dapibus ultrices. Pretium lectus quam id leo in vitae turpis massa. Malesuada bibendum arcu vitae elementum curabitur vitae nunc sed velit. Imperdiet massa tincidunt nunc pulvinar sapien et ligula. Quam viverra orci sagittis eu. Mi sit amet mauris commodo. Sit amet nisl purus in mollis nunc sed. Mattis vulputate enim nulla aliquet porttitor lacus luctus accumsan tortor. Viverra adipiscing at in tellus integer feugiat scelerisque. Egestas congue quisque egestas diam in arcu cursus euismod. Neque sodales ut etiam sit amet. Et tortor consequat id porta nibh venenatis cras sed. Tellus molestie nunc non blandit massa enim nec dui nunc. Sed nisi lacus sed viverra tellus. Amet nisl suscipit adipiscing bibendum est ultricies integer. Lorem dolor sed viverra ipsum nunc aliquet bibendum enim facilisis.
<br>
<br>
<br>

## Results:

### Link Breakage from Spine to Leaf and from Leaf to ToR - Result          

<br>
<br>

### Inter & Intra Pod Travel Time - Result          
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Commodo sed egestas egestas fringilla phasellus faucibus. Nulla malesuada pellentesque elit eget. Integer eget aliquet nibh praesent tristique magna sit amet purus. In eu mi bibendum neque egestas congue quisque. Fermentum et sollicitudin ac orci phasellus. Purus sit amet volutpat consequat mauris nunc congue. Morbi enim nunc faucibus a pellentesque sit amet. Turpis cursus in hac habitasse platea dictumst quisque sagittis purus. Ut enim blandit volutpat maecenas volutpat. Duis at consectetur lorem donec massa sapien faucibus et. Aliquam malesuada bibendum arcu vitae elementum curabitur vitae nunc sed. Risus nullam eget felis eget nunc.

Pharetra convallis posuere morbi leo urna molestie at. Condimentum mattis pellentesque id nibh tortor id aliquet lectus. Massa id neque aliquam vestibulum morbi blandit cursus. Sed libero enim sed faucibus turpis. Pellentesque id nibh tortor id aliquet lectus proin. Et egestas quis ipsum suspendisse ultrices gravida dictum. Et egestas quis ipsum suspendisse ultrices gravida. Imperdiet proin fermentum leo vel orci porta non pulvinar. Nam aliquam sem et tortor consequat id porta nibh venenatis. Mauris rhoncus aenean vel elit scelerisque. Vulputate enim nulla aliquet porttitor lacus.
<br>
<br>

### Expand by One Spine, Leaf, and ToR - Result         
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Quis viverra nibh cras pulvinar mattis nunc. Eget arcu dictum varius duis at consectetur. Vitae semper quis lectus nulla at. Sit amet luctus venenatis lectus. Mattis molestie a iaculis at erat pellentesque adipiscing. Convallis posuere morbi leo urna molestie at elementum eu. Amet cursus sit amet dictum sit. Nibh praesent tristique magna sit amet purus gravida quis blandit. Ullamcorper morbi tincidunt ornare massa eget egestas purus viverra. Ultrices eros in cursus turpis massa. Dictum at tempor commodo ullamcorper a lacus vestibulum sed. Mattis nunc sed blandit libero volutpat sed cras ornare. Ac feugiat sed lectus vestibulum mattis ullamcorper velit sed ullamcorper.

Dolor magna eget est lorem ipsum dolor sit. Vel facilisis volutpat est velit egestas dui id ornare arcu. Augue eget arcu dictum varius duis at consectetur lorem. Eu non diam phasellus vestibulum lorem sed risus ultricies. In iaculis nunc sed augue lacus viverra vitae. Ultricies leo integer malesuada nunc vel risus commodo viverra. Pellentesque adipiscing commodo elit at imperdiet dui accumsan sit. Gravida rutrum quisque non tellus orci. Sollicitudin aliquam ultrices sagittis orci a scelerisque purus semper eget. Enim sit amet venenatis urna cursus eget nunc. Mauris a diam maecenas sed enim ut sem viverra. Hendrerit gravida rutrum quisque non tellus orci ac auctor. Quam nulla porttitor massa id neque. A diam sollicitudin tempor id eu nisl nunc. Auctor eu augue ut lectus arcu bibendum.
<br>
<br>

### Deploy a New Server - Result            
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Scelerisque viverra mauris in aliquam. Pharetra massa massa ultricies mi. Mattis molestie a iaculis at erat pellentesque adipiscing. Ullamcorper morbi tincidunt ornare massa. Accumsan in nisl nisi scelerisque eu ultrices. Nisi quis eleifend quam adipiscing vitae proin sagittis nisl. Elit duis tristique sollicitudin nibh. Iaculis nunc sed augue lacus viverra vitae congue eu consequat. Sem fringilla ut morbi tincidunt augue interdum. Duis at tellus at urna condimentum mattis pellentesque. Diam sit amet nisl suscipit adipiscing bibendum est ultricies integer. Et leo duis ut diam quam nulla porttitor. Ac orci phasellus egestas tellus rutrum tellus pellentesque. Amet facilisis magna etiam tempor orci eu lobortis elementum nibh. Tristique sollicitudin nibh sit amet commodo nulla.

A lacus vestibulum sed arcu non odio euismod. Neque laoreet suspendisse interdum consectetur libero id faucibus. Elementum nibh tellus molestie nunc non blandit massa enim nec. Suspendisse potenti nullam ac tortor vitae. Nisl purus in mollis nunc sed id semper. Eros in cursus turpis massa tincidunt dui ut. Commodo ullamcorper a lacus vestibulum sed arcu non odio euismod. Dolor sit amet consectetur adipiscing elit. Iaculis urna id volutpat lacus laoreet. Consectetur lorem donec massa sapien faucibus. Tristique magna sit amet purus gravida quis. Sit amet dictum sit amet. Ultrices gravida dictum fusce ut placerat orci. Id venenatis a condimentum vitae sapien pellentesque. Odio pellentesque diam volutpat commodo. Lectus mauris ultrices eros in cursus turpis. Posuere morbi leo urna molestie at elementum eu facilisis sed. Est placerat in egestas erat imperdiet sed euismod nisi.
<br>
<br>
<br>

## Discussion of results:           
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Nulla aliquet enim tortor at auctor. Diam vulputate ut pharetra sit amet aliquam id. Nulla porttitor massa id neque aliquam vestibulum morbi blandit. Volutpat sed cras ornare arcu. Sapien et ligula ullamcorper malesuada proin libero nunc consequat. Tortor id aliquet lectus proin nibh nisl. Morbi non arcu risus quis varius quam quisque id diam. Aliquam purus sit amet luctus venenatis. Massa vitae tortor condimentum lacinia quis. Porta lorem mollis aliquam ut porttitor leo a. Donec ultrices tincidunt arcu non sodales. Nisl tincidunt eget nullam non nisi est sit amet facilisis. Eu consequat ac felis donec. Nulla aliquet enim tortor at auctor. At tempor commodo ullamcorper a lacus vestibulum sed arcu non. Imperdiet proin fermentum leo vel. At consectetur lorem donec massa sapien faucibus. Quam nulla porttitor massa id neque aliquam vestibulum. Quis imperdiet massa tincidunt nunc pulvinar sapien.

Aliquam ultrices sagittis orci a. Adipiscing commodo elit at imperdiet dui accumsan sit. Cras sed felis eget velit aliquet. Cras tincidunt lobortis feugiat vivamus at. Faucibus nisl tincidunt eget nullam non nisi est sit amet. Egestas dui id ornare arcu odio. Odio euismod lacinia at quis risus sed vulputate. Faucibus in ornare quam viverra orci sagittis eu. Neque vitae tempus quam pellentesque. Eget magna fermentum iaculis eu non diam. Eget nulla facilisi etiam dignissim diam quis enim lobortis scelerisque. Feugiat nibh sed pulvinar proin gravida. Bibendum arcu vitae elementum curabitur. Eu scelerisque felis imperdiet proin fermentum leo vel orci porta. Quis hendrerit dolor magna eget est. Neque vitae tempus quam pellentesque nec nam. Mi eget mauris pharetra et ultrices neque. Nibh tortor id aliquet lectus proin nibh nisl condimentum. Vel turpis nunc eget lorem dolor sed. Faucibus et molestie ac feugiat sed lectus vestibulum.
<br>
<br>
<br>

## Critical Summary:            
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Aliquam faucibus purus in massa tempor nec feugiat nisl. Nulla aliquet porttitor lacus luctus accumsan tortor posuere ac ut. Sed viverra ipsum nunc aliquet bibendum. Nisl pretium fusce id velit ut tortor pretium. Risus pretium quam vulputate dignissim suspendisse in. Commodo nulla facilisi nullam vehicula ipsum. Commodo ullamcorper a lacus vestibulum sed arcu. Elementum nibh tellus molestie nunc. Venenatis lectus magna fringilla urna. Interdum posuere lorem ipsum dolor sit amet consectetur adipiscing. Nisi lacus sed viverra tellus in hac habitasse platea.

Libero id faucibus nisl tincidunt eget nullam non. Elementum sagittis vitae et leo duis. Mus mauris vitae ultricies leo integer malesuada nunc vel risus. Mi in nulla posuere sollicitudin aliquam ultrices sagittis orci. Fermentum posuere urna nec tincidunt. Vivamus arcu felis bibendum ut tristique et egestas quis. Quis imperdiet massa tincidunt nunc pulvinar sapien et ligula ullamcorper. Aliquet nibh praesent tristique magna sit amet purus gravida. In hendrerit gravida rutrum quisque non. Augue neque gravida in fermentum et sollicitudin ac orci phasellus. Maecenas volutpat blandit aliquam etiam erat velit scelerisque in. Amet nisl suscipit adipiscing bibendum est ultricies integer. Pulvinar mattis nunc sed blandit. Interdum varius sit amet mattis vulputate enim. Sit amet purus gravida quis blandit turpis cursus in hac. Odio ut sem nulla pharetra diam sit amet nisl. Lacinia at quis risus sed vulputate odio ut enim blandit. Rhoncus mattis rhoncus urna neque viverra justo nec ultrices.
<br>
<br>
<br>

## References:          
Dutt, Dinesh. BGP in the Data Center. 1st ed., Van Duuren Media, 2017.          
Zhang, Randy, and Micah Bartell. BGP Design and Implementation. Amsterdam University Press, 2004.           
Camarero, Cristobal, et al. “Random Folded Clos Topologies for Datacenter Networks.” 2017 IEEE International Symposium on High Performance Computer Architecture (HPCA), 2017. Crossref, https://doi.org/10.1109/hpca.2017.26.                  
Garcia-Martinez, Alberto, and Marcelo Bagnulo. “Measuring BGP Route Propagation Times.” IEEE Communications Letters, vol. 23, no. 12, 2019, pp. 2432–36. Crossref, https://doi.org/10.1109/lcomm.2019.2945964.                      



Figure 1 - By Jafet at English Wikipedia - Transferred from en.wikipedia to Commons., Public Domain, https://commons.wikimedia.org/w/index.php?curid=37953156          
Figure 2 - Public Domain, https://commons.wikimedia.org/w/index.php?curid=991408                    
Figure 3 - By Original: FoobazSVG: Rehua - This file was derived from: NetworkTopology-Mesh.png, Public Domain, https://commons.wikimedia.org/w/index.php?curid=31012050

<br>
<br>
<br>

## Appendix:            
LSA 