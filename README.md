6


# Set simulation parameters
set val(chan) Channel/WirelessChannel
set val(prop) Propagation/TwoRayGround
set val(netif) Phy/WirelessPhy
set val(mac) Mac/802_11
set val(ifq) Queue/DropTail/PriQueue
set val(ll) LL
set val(ant) Antenna/OmniAntenna
set val(x) 500
set val(y) 500
set val(ifqlen) 50
set val(nn) 2  ;# number of nodes
set val(stop) 20.0  ;# simulation duration (seconds)
set val(rp) DSDV  ;# routing protocol (DSDV)

# Initialize the simulator
set ns [new Simulator]

# Create trace files for simulation
set tracefd [open 001.tr w]
$ns trace-all $tracefd
set namtrace [open 001.nam w]
$ns namtrace-all-wireless $namtrace $val(x) $val(y)

# Set up propagation model and topology
set prop [new $val(prop)]
set topo [new Topography]
$topo load_flatgrid $val(x) $val(y)

# Create the "God" (for simulation nodes)
create-god $val(nn)

# Configure the nodes
$ns node-config -adhocRouting $val(rp) \
                -llType $val(ll) \
                -macType $val(mac) \
                -ifqType $val(ifq) \
                -ifqLen $val(ifqlen) \
                -antType $val(ant) \
                -propType $val(prop) \
                -phyType $val(netif) \
                -channelType $val(chan) \
                -topoInstance $topo \
                -agentTrace ON \
                -routerTrace ON \
                -macTrace ON

# Create nodes and enable random motion
for {set i 0} { $i < $val(nn) } { incr i } {
    set node_($i) [$ns node]
    $node_($i) random-motion 0
}

# Set initial positions for the nodes
for {set i 0} { $i < $val(nn) } { incr i } {
    $ns initial_node_pos $node_($i) 40  ;# Set random initial positions
}

# Set destination for the nodes at time 1.1 seconds
$ns at 1.1 "$node_(0) setdest 310.0 10.0 20.0"
$ns at 1.1 "$node_(1) setdest 10.0 310.0 20.0"

# Generating TCP traffic
set tcp0 [new Agent/TCP]
set sink0 [new Agent/TCPSink]
$ns attach-agent $node_(0) $tcp0
$ns attach-agent $node_(1) $sink0
$ns connect $tcp0 $sink0

# Attach FTP application to the TCP agent
set ftp0 [new Application/FTP]
$ftp0 attach-agent $tcp0

# Start and stop FTP traffic
$ns at 1.0 "$ftp0 start"
$ns at 18.0 "$ftp0 stop"

# Reset nodes at the end of the simulation
for {set i 0} { $i < $val(nn) } { incr i } {
    $ns at $val(stop) "$node_($i) reset"
}

# Output message to indicate simulation start
puts "Starting simulation..."

# Schedule the end of the simulation and output exit message
$ns at $val(stop) "puts \"NS EXITING...\"; finish"

# Define the finish procedure to end the simulation, close files, and run NAM
proc finish {} {
    global ns tracefd namtrace
    $ns flush-trace
    close $tracefd
    close $namtrace
    exec nam 001.nam &  ;# Start NAM to visualize the simulation
    exit 0
}

# Run the simulation
$ns run


//awk

BEGIN {
recd=0
hdrsz=0
stoptime=0
starttime=0
}

{
time=$2
if($1=="s" &&  $4=="AGT" && $8>=512) {
if(time<starttime) {
starttime=time
}
}

if($1=="r" &&  $4=="AGT" && $8>=512) {
if(time>starttime) {
stoptime=time
}
hdrsz=$8%512
$8-=hdrsz
recd+=$8
}
}
END {
printf("Goodput=%f Kbps\n", (recd)/(stoptime-starttime)*8/1000)
}



7



set val(chan) Channel/WirelessChannel
set val(prop) Propagation/TwoRayGround
set val(netif) Phy/WirelessPhy
set val(mac) Mac/802_11
set val(ifq) Queue/DropTail/PriQueue
set val(ll) LL
set val(ant) Antenna/OmniAntenna
set val(x) 500
set val(y) 400
set val(ifqlen) 50
set val(nn) 3
set val(stop) 60.0
set val(rp) AODV
set ns_ [new Simulator]
set tracefd [open 002.tr w]
$ns_ trace-all $tracefd
set namtrace [open 002.nam w]
$ns_ namtrace-all-wireless $namtrace $val(x) $val(y)
set prop [new $val(prop)]
set topo [new Topography]
$topo load_flatgrid $val(x) $val(y)
create-god $val(nn)
$ns_ node-config -adhocRouting $val(rp) \
-llType $val(ll) \
-macType $val(mac) \
-ifqType $val(ifq) \
-ifqLen $val(ifqlen) \
-antType $val(ant) \
-propType $val(prop) \
-phyType $val(netif) \
-channelType $val(chan) \
-topoInstance $topo \
-agentTrace ON \
-routerTrace ON \
-macTrace ON
for {set i 0} {$i < $val(nn)} {incr i} {
 set node_($i) [$ns_ node]
 $node_($i) random-motion 0
}
$node_(0) set x_ 5.0
$node_(0) set y_ 5.0
$node_(0) set z_ 0.0
$node_(1) set x_ 490.0

$node_(1) set y_ 285.0

$node_(1) set z_ 0.0
$node_(2) set x_ 150.0
$node_(2) set y_ 240.0
$node_(2) set z_ 0.0
for {set i 0} {$i < $val(nn)} {incr i} {
 $ns_ initial_node_pos $node_($i) 40
}
$ns_ at 0.0 "$node_(0) setdest 450.0 285.0 30.0"
$ns_ at 0.0 "$node_(1) setdest 200.0 285.0 30.0"
$ns_ at 0.0 "$node_(2) setdest 1.0 285.0 30.0"
$ns_ at 25.0 "$node_(0) setdest 300.0 285.0 10.0"
$ns_ at 25.0 "$node_(2) setdest 100.0 285.0 10.0"
$ns_ at 40.0 "$node_(0) setdest 490.0 285.0 5.0"
$ns_ at 40.0 "$node_(2) setdest 1.0 285.0 5.0"
set tcp0 [new Agent/TCP]
set sink0 [new Agent/TCPSink]
$ns_ attach-agent $node_(0) $tcp0
$ns_ attach-agent $node_(2) $sink0
$ns_ connect $tcp0 $sink0
set ftp0 [new Application/FTP]
$ftp0 attach-agent $tcp0
$ns_ at 10.0 "$ftp0 start"
for {set i 0} {$i < $val(nn)} {incr i} {
 $ns_ at $val(stop) "$node_($i) reset"
}
$ns_ at $val(stop) "puts \"NS EXITING...\" ; finish"
puts "Starting Simulation..."
proc finish {} {
 global ns_ tracefd namtrace
 $ns_ flush-trace
 close $tracefd
 close $namtrace
 exec nam 002.nam &
 exit 0
}
$ns_ run 


//awk

BEGIN {
recd=0
hdrsz=0
stoptime=0
starttime=0
}

{
time=$2
if($1=="s" &&  $4=="AGT" && $8>=512) {
if(time<starttime) {
starttime=time
}
}

if($1=="r" &&  $4=="AGT" && $8>=512) {
if(time>starttime) {
stoptime=time
}
hdrsz=$8%512
$8-=hdrsz
recd+=$8
}
}
END {
printf("Goodput=%f Kbps\n", (recd)/(stoptime-starttime)*8/1000)
}














8



set val(chan) Channel/WirelessChannel
set val(prop) Propagation/TwoRayGround
set val(netif) Phy/WirelessPhy
set val(mac) Mac/802_11
set val(ifq) Queue/DropTail/PriQueue
set val(ll) LL
set val(ant) Antenna/OmniAntenna
set val(x) 500
set val(y) 500
set val(ifqlen) 50
set val(nn) 50
set val(stop) 100.0
set val(rp) AODV
set val(sc) "transmit"
set val(cp) "mobile"
set ns_ [new Simulator]
set tracefd [open 003.tr w]
$ns_ trace-all $tracefd
set namtrace [open 003.nam w]
$ns_ namtrace-all-wireless $namtrace $val(x) $val(y)
set prop [new $val(prop)]
set topo [new Topography]
$topo load_flatgrid $val(x) $val(y)
set god_ [create-god $val(nn)]
#Node Configuration
$ns_ node-config -adhocRouting $val(rp) \
-llType $val(ll) \
-macType $val(mac) \
-ifqType $val(ifq) \
-ifqLen $val(ifqlen) \
-antType $val(ant) \
-propType $val(prop) \
-phyType $val(netif) \
-channelType $val(chan) \
-topoInstance $topo \
-agentTrace ON \
-routerTrace ON \
-macTrace ON
#Creating Nodes
for {set i 0} {$i < $val(nn)} {incr i} {
set node_($i) [$ns_ node]
$node_($i) random-motion 0
}
for {set i 0} {$i < $val(nn)} {incr i} {
set xx [expr rand()*500]
set yy [expr rand()*500]
$node_($i) set X_ $xx
$node_($i) set Y_ $yy
}
#Initial Positions of Nodes
for {set i 0} {$i < $val(nn)} {incr i} {
$ns_ initial_node_pos $node_($i) 40
}
#puts "Loading scenario file..."
source $val(sc)
puts "Loading connection file..."
source $val(cp)
#Simulation Termination
for {set i 0} {$i < $val(nn)} {incr i} {
$ns_ at $val(stop) "$node_($i) reset";
}
$ns_ at $val(stop) "puts \"NS EXITING...\" ; $ns_ halt"
puts "Starting Simulation..."
$ns_ run


//awk

BEGIN {
recd=0
hdrsz=0
stoptime=0
starttime=0
}

{
time=$2
if($1=="s" &&  $4=="AGT" && $8>=512) {
if(time<starttime) {
starttime=time
}
}

if($1=="r" &&  $4=="AGT" && $8>=512) {
if(time>starttime) {
stoptime=time
}
hdrsz=$8%512
$8-=hdrsz
recd+=$8
}
}
END {
printf("Goodput=%f Kbps\n", (recd)/(stoptime-starttime)*8/1000)
}




9



set val(chan) Channel/WirelessChannel
set val(prop) Propagation/TwoRayGround 
set val(netif) Phy/WirelessPhy
set val(mac) Mac/802_11
set val(ifq) CMUPriQueue
set val(ll) LL
set val(ant) Antenna/OmniAntenna 
set val(x) 700
set val(y) 700
set val(ifqlen) 50
set val(nn) 6
set val(stop) 60.0
set val(rp) DSR

set ns_ [new Simulator]
set tracefd [open 004.tr w]
$ns_ trace-all $tracefd
set namtrace [open 004.nam w]
$ns_ namtrace-all-wireless $namtrace $val(x) $val(y)

set prop [new $val(prop)] 
set topo [new Topography]
$topo load_flatgrid $val(x) $val(y) 

set god_ [create-god $val(nn)]

# Node Configuration
$ns_ node-config -adhocRouting $val(rp) \
-llType $val(ll) \
-macType $val(mac) \
-ifqType $val(ifq) \
-ifqLen $val(ifqlen) \
-antType $val(ant) \
-propType $val(prop) \
-phyType $val(netif) \
-channelType $val(chan) \
-topoInstance $topo \
-agentTrace ON \
-routerTrace ON \
-macTrace ON

# Creating Nodes
for {set i 0} {$i < $val(nn)} {incr i} { 
    set node_($i) [$ns_ node]
    $node_($i) random-motion 0
}

# Initial Positions of Nodes
$node_(0) set X_ 150.0
$node_(0) set Y_ 300.0
$node_(0) set Z_ 0.0
$node_(1) set X_ 300.0
$node_(1) set Y_ 500.0
$node_(1) set Z_ 0.0
$node_(2) set X_ 500.0
$node_(2) set Y_ 500.0
$node_(2) set Z_ 0.0
$node_(3) set X_ 300.0
$node_(3) set Y_ 100.0
$node_(3) set Z_ 0.0
$node_(4) set X_ 500.0
$node_(4) set Y_ 100.0
$node_(4) set Z_ 0.0
$node_(5) set X_ 650.0
$node_(5) set Y_ 300.0
$node_(5) set Z_ 0.0

for {set i 0} {$i < $val(nn)} {incr i} {
    $ns_ initial_node_pos $node_($i) 40
}

# Topology Design
$ns_ at 1.0 "$node_(0) setdest 160.0 300.0 2.0"
$ns_ at 1.0 "$node_(1) setdest 310.0 150.0 2.0"
$ns_ at 1.0 "$node_(2) setdest 490.0 490.0 2.0"
$ns_ at 1.0 "$node_(3) setdest 300.0 120.0 2.0"
$ns_ at 1.0 "$node_(4) setdest 510.0 90.0 2.0"
$ns_ at 1.0 "$node_(5) setdest 640.0 290.0 2.0"
$ns_ at 4.0 "$node_(3) setdest 300.0 500.0 5.0" 

# Generating Traffic
set tcp0 [new Agent/TCP]
set sink0 [new Agent/TCPSink]

$ns_ attach-agent $node_(0) $tcp0
$ns_ attach-agent $node_(5) $sink0
$ns_ connect $tcp0 $sink0

set ftp0 [new Application/FTP]
$ftp0 attach-agent $tcp0

$ns_ at 5.0 "$ftp0 start"
$ns_ at 60.0 "$ftp0 stop"

# Simulation Termination
for {set i 0} {$i < $val(nn)} {incr i} {
    $ns_ at $val(stop) "$node_($i) reset"
}

$ns_ at $val(stop) "puts \"NS EXITING...\" ; $ns_ halt"

puts "Starting Simulation..."
$ns_ run



//awk

BEGIN {
recd=0
hdrsz=0
stoptime=0
starttime=0
}

{
time=$2
if($1=="s" &&  $4=="AGT" && $8>=512) {
if(time<starttime) {
starttime=time
}
}

if($1=="r" &&  $4=="AGT" && $8>=512) {
if(time>starttime) {
stoptime=time
}
hdrsz=$8%512
$8-=hdrsz
recd+=$8
}
}
END {
printf("Goodput=%f Kbps\n", (recd)/(stoptime-starttime)*8/1000)
}









10



set val(chan) Channel/WirelessChannel
set val(prop) Propagation/TwoRayGround
set val(netif) Phy/WirelessPhy
set val(mac) Mac/802_11
set val(ifq) Queue/DropTail/PriQueue
set val(ll) LL
set val(ant) Antenna/OmniAntenna
set val(x) 500
set val(y) 500
set val(ifqlen) 50
set val(nn) 5
set val(stop) 50.0
set val(rp) AODV
set ns_ [new Simulator]
set tracefd [open 005.tr w]
$ns_ trace-all $tracefd
set namtrace [open 005.nam w]
$ns_ namtrace-all-wireless $namtrace $val(x) $val(y)
set cwind1 [open win1.tr w]
set cwind2 [open win2.tr w]

set prop [new $val(prop)]
set topo [new Topography]
$topo load_flatgrid $val(x) $val(y)
create-god $val(nn)
$ns_ node-config -adhocRouting $val(rp) \
-llType $val(ll) \
-macType $val(mac) \
-ifqType $val(ifq) \
-ifqLen $val(ifqlen) \
-antType $val(ant) \
-propType $val(prop) \
-phyType $val(netif) \
-channelType $val(chan) \
-topoInstance $topo \
-agentTrace ON \
-routerTrace ON \
-macTrace ON \
-IncomingErrProc "uniformErr" \
-OutgoingErrProc "uniformErr"
proc uniformErr {} {
set err [new ErrorModel]
$err unit pkt
$err set rate_ 0.20
return $err
}
for {set i 0} {$i < $val(nn)} {incr i} {
set node_($i) [$ns_ node]
$node_($i) random-motion 0
}

for {set i 0} {$i < $val(nn)} {incr i} {
$ns_ initial_node_pos $node_($i) 40
}
$ns_ at 1.0 "$node_(0) setdest 10.0 10.0 50.0"
$ns_ at 1.0 "$node_(1) setdest 10.0 100.0 50.0"
$ns_ at 1.0 "$node_(4) setdest 50.0 50.0 50.0"
$ns_ at 1.0 "$node_(2) setdest 100.0 100.0 50.0"
$ns_ at 1.0 "$node_(3) setdest 100.0 10.0 50.0"
set tcp0 [new Agent/TCP]
set sink0 [new Agent/TCPSink]
$ns_ attach-agent $node_(0) $tcp0
$ns_ attach-agent $node_(2) $sink0
$ns_ connect $tcp0 $sink0
set ftp0 [new Application/FTP]
$ftp0 attach-agent $tcp0
$ns_ at 1.0 "$ftp0 start"
$ns_ at $val(stop) "$ftp0 stop"
set tcp1 [new Agent/TCP]
set sink1 [new Agent/TCPSink]
$ns_ attach-agent $node_(1) $tcp1
$ns_ attach-agent $node_(2) $sink1
$ns_ connect $tcp1 $sink1
set ftp1 [new Application/FTP]
$ftp1 attach-agent $tcp1
$ns_ at 1.0 "$ftp1 start"
$ns_ at $val(stop) "$ftp1 stop"
proc plotWindow {tcpSource file} {
global ns_
set time 0.01
set now [$ns_ now]
set cwind [$tcpSource set cwnd_]
puts $file "$now $cwind"
$ns_ at [expr $now + $time ] "plotWindow $tcpSource $file"
}
$ns_ at 2.0 "plotWindow $tcp0 $cwind1"
$ns_ at 2.0 "plotWindow $tcp1 $cwind2"
$ns_ at 50.0 "finish"
for {set i 0} {$i < $val(nn)} {incr i} {
$ns_ at $val(stop) "$node_($i) reset"
}
$ns_ at $val(stop) "puts \"NS EXITING...\" ; $ns_ halt"
puts "Starting Simulation..."
proc finish {} {
global ns_ tracefd namtrace
$ns_ flush-trace
close $tracefd
close $namtrace
exec nam 005.nam &

exec xgraph win1.tr &
exec xgraph win2.tr &
exit 0
}
$ns_ run

//awk

BEGIN {
recd=0
hdrsz=0
stoptime=0
starttime=0
}

{
time=$2
if($1=="s" &&  $4=="AGT" && $8>=512) {
if(time<starttime) {
starttime=time
}
}

if($1=="r" &&  $4=="AGT" && $8>=512) {
if(time>starttime) {
stoptime=time
}
hdrsz=$8%512
$8-=hdrsz
recd+=$8
}
}
END {
printf("Goodput=%f Kbps\n", (recd)/(stoptime-starttime)*8/1000)
}



