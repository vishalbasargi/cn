PROGRAM 6:

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
set val(nn) 2
set val(stop) 20.0
set val(rp) DSDV

set ns [new Simulator]
set tracefd [open p6.tr w]
$ns trace-all $tracefd
set namtrace [open p6.nam w]
$ns namtrace-all-wireless $namtrace $val(x) $val(y)
set prop [new $val(prop)]
set topo [new Topography]
$topo load_flatgrid $val(x) $val(y)
create-god $val(nn)
$ns node-config -adhocRouting $val(rp)\
-llType $val(ll)\
-macType $val(mac)\
-ifqType $val(ifq)\
-ifqLen $val(ifqlen)\
-antType $val(ant)\
-propType $val(prop)\
-phyType $val(netif)\
-channelType $val(chan)\
-topoInstance $topo\
-agentTrace ON\
-routerTrace ON\
-macTrace ON\

for {set i 0} { $i< $val(nn)} { incr i} {
set node_($i) [$ns node]
$node_($i) random-motion 0
}

for {set i 0} { $i< $val(nn) } {incr i} {
$ns initial_node_pos $node_($i) 40
}
$ns at 1.1 "$node_(0) setdest 310.0 10.0 20.0"
$ns at 1.1 "$node_(1) setdest 10.0 310.0 20.0"

set tcp0 [new Agent/TCP]
set sink0 [new Agent/TCPSink]
$ns attach-agent $node_(0) $tcp0
$ns attach-agent $node_(1) $sink0
$ns connect $tcp0 $sink0
set ftp0 [new Application/FTP]
$ftp0 attach-agent $tcp0
$ns at 1.0 "$ftp0 start"
$ns at 18.0 "$ftp0 stop"
for {set i 0} { $i<$val(nn)} { incr i} {
$ns at $val(stop) "$node_($i) reset";
}

puts "starting simulation"
$ns at $val(stop) "puts \"NS EXITING...\"; finish"
proc finish {} {
global ns tracefd namtrace
$ns flush-trace
close $tracefd
close $namtrace
exec nam p6.nam &
exit 0




PROGRAM 7:

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
for {set i 0} {$i < $val(nn) } {incr i} {
set node_($i) [$ns_ node]
$node_($i) random-motion 0
}
#Initial Positions of Nodes
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
#Topology Design
$ns_ at 0.0 "$node_(0) setdest 450.0 285.0 30.0"
$ns_ at 0.0 "$node_(1) setdest 200.0 285.0 30.0"
$ns_ at 0.0 "$node_(2) setdest 1.0 285.0 30.0"
$ns_ at 25.0 "$node_(0) setdest 300.0 285.0 10.0"
$ns_ at 25.0 "$node_(2) setdest 100.0 285.0 10.0"
$ns_ at 40.0 "$node_(0) setdest 490.0 285.0 5.0"
$ns_ at 40.0 "$node_(2) setdest 1.0 285.0 5.0"

#Generating Traffic
set tcp0 [new Agent/TCP]
set sink0 [new Agent/TCPSink]
$ns_ attach-agent $node_(0) $tcp0
$ns_ attach-agent $node_(2) $sink0
$ns_ connect $tcp0 $sink0
set ftp0 [new Application/FTP]
$ftp0 attach-agent $tcp0
$ns_ at 10.0 "$ftp0 start"
#Simulation Termination
for {set i 0} {$i < $val(nn) } {incr i} {
$ns_ at $val(stop) "$node_($i) reset";
}
proc finish {} {
global ns_ tracefd namtrace
$ns_ flush-trace
close $tracefd
close $namtrace
exec nam 002.nam &
exit 0
}
$ns_ at $val(stop) "puts \"NS EXITING...\" ; finish; $ns_ halt"
puts "Starting Simulation..."
$ns_ run





PROGRAM 8:

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
for {set i 0} {$i < $val(nn) } {incr i} {
set node_($i) [$ns_ node]
$node_($i) random-motion 0
}
for {set i 0} {$i < $val(nn) } { incr i } {
set xx [expr rand()*500]
set yy [expr rand()*400]
$node_($i) set X_ $xx
$node_($i) set Y_ $yy
}
for {set i 0} {$i < $val(nn)} {incr i} {

$ns_ initial_node_pos $node_($i) 40
}
source $val(sc)
puts "Loading connection file..."
source $val(cp)
for {set i 0} {$i < $val(nn) } {incr i} {
$ns_ at $val(stop) "$node_($i) reset";
}
$ns_ at $val(stop) "puts \"NS EXITING...\" ; $ns_ halt"
puts "Starting Simulation..."
$ns_ run


program 9

# Set simulation parameters
set val(chan) Channel/WirelessChannel
set val(prop) Propagation/TwoRayGround
set val(netif) Phy/WirelessPhy
set val(mac) Mac/802_11
#set val(ifq) Queue/DropTail/PriQueue
set val(ifq) CMUPriQueue
set val(ll) LL
set val(ant) Antenna/OmniAntenna
set val(x) 700
set val(y) 700
set val(ifqlen) 50
set val(nn) 6
set val(stop) 60.0
set val(rp) DSR
# Initialize simulator
set ns_ [new Simulator]
set tracefd [open 004.tr w]
$ns_ trace-all $tracefd
set namtrace [open 004.nam w]
$ns_ namtrace-all-wireless $namtrace $val(x) $val(y)
# Set propagation model
set prop [new $val(prop)]
set topo [new Topography]
$topo load_flatgrid $val(x) $val(y)
# Initialize the General Operations Director (GOD)
set god_ [create-god $val(nn)]
# Node configuration
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
# Create nodes
for {set i 0} {$i < $val(nn)} {incr i} {
set node_($i) [$ns_ node]
$node_($i) random-motion 0
}
# Initial positions of nodes
$node_(0) set X_ 150.0
$node_(0) set Y_ 300.0
$node_(0) set Z_ 0.0
$node_(1) set X_ 300.0
$node_(1) set Y_ 500.0
$node_(1) set Z_ 0.0
$node_(2) set X_ 500.0
$node_(2) set Y_ 500.0
$node_(2) set Z_ 0.0 ;# Fixed typo: $nde_(2) -> $node_(2)
$node_(3) set X_ 300.0
$node_(3) set Y_ 100.0
$node_(3) set Z_ 0.0
$node_(4) set X_ 500.0
$node_(4) set Y_ 100.0
$node_(4) set Z_ 0.0
$node_(5) set X_ 650.0
$node_(5) set Y_ 300.0
$node_(5) set Z_ 0.0

# Set initial positions for nodes in the simulator
for {set i 0} {$i < $val(nn)} {incr i} {
$ns_ initial_node_pos $node_($i) 40
}
# Topology design
$ns_ at 1.0 "$node_(0) setdest 160.0 300.0 2.0"
$ns_ at 1.0 "$node_(1) setdest 310.0 150.0 2.0"
$ns_ at 1.0 "$node_(2) setdest 490.0 490.0 2.0"
$ns_ at 1.0 "$node_(3) setdest 300.0 120.0 2.0"
$ns_ at 1.0 "$node_(4) setdest 510.0 90.0 2.0"
$ns_ at 1.0 "$node_(5) setdest 640.0 290.0 2.0"
$ns_ at 4.0 "$node_(3) setdest 300.0 500.0 5.0"
# Generate traffic
set tcp0 [new Agent/TCP]
set sink0 [new Agent/TCPSink]
$ns_ attach-agent $node_(0) $tcp0
$ns_ attach-agent $node_(5) $sink0
$ns_ connect $tcp0 $sink0
set ftp0 [new Application/FTP]
$ftp0 attach-agent $tcp0
$ns_ at 5.0 "$ftp0 start"
$ns_ at 60.0 "$ftp0 stop"
# Simulation termination
for {set i 0} {$i < $val(nn)} {incr i} {
$ns_ at $val(stop) "$node_($i) reset"
}
$ns_ at $val(stop) "puts \"NS EXITING...\" ; $ns_ halt"
# Start simulation
puts "Starting Simulation..."
proc finish {} {
global ns_ tracefd namtrace
$ns_ flush-trace
close $tracefd
close $namtrace
exec nam 004.nam &
exit 0
}
$ns_ run




Program 10:

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
set val(nn) 5
set val(stop) 50.0
set val(rp) AODV
# Initialize simulator
set ns_ [new Simulator]
set tracefd [open 005.tr w]
$ns_ trace-all $tracefd
set namtrace [open 005.nam w]
$ns_ namtrace-all-wireless $namtrace $val(x) $val(y)
set cwind [open win5.tr w]
# Set propagation model
set prop [new $val(prop)]
set topo [new Topography]
$topo load_flatgrid $val(x) $val(y)
# Initialize General Operations Director (GOD)
create-god $val(nn)
# Node configuration
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
# Define error model procedure
proc uniformErr {} {
set err [new ErrorModel]
$err unit pkt
$err set rate_ 0.20
return $err
}

# Create nodes
for {set i 0} {$i < $val(nn)} {incr i} {
set node_($i) [$ns_ node]
$node_($i) random-motion 0
}
# Initial positions of nodes
for {set i 0} {$i < $val(nn)} {incr i} {
$ns_ initial_node_pos $node_($i) 40
}
# Topology design
$ns_ at 1.0 "$node_(0) setdest 10.0 10.0 50.0"
$ns_ at 1.0 "$node_(1) setdest 10.0 100.0 50.0"
$ns_ at 1.0 "$node_(4) setdest 50.0 50.0 50.0"
$ns_ at 1.0 "$node_(2) setdest 100.0 100.0 50.0"
$ns_ at 1.0 "$node_(3) setdest 100.0 10.0 50.0"
# Generating traffic
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
# Simulation termination
for {set i 0} {$i < $val(nn)} {incr i} {
$ns_ at $val(stop) "$node_($i) reset"
}
$ns_ at $val(stop) "puts \"NS EXITING...\" ; $ns_ halt"

proc plotWindow {tcpSource file} {
global ns_
set time 0.01
set now [$ns_ now]
set cwnd [$tcpSource set cwnd_]
puts $file "$now $cwnd"
$ns_ at [expr $now + $time] "plotWindow $tcpSource $file"}
$ns_ at 1.0 "plotWindow $tcp0 $cwind"


# Start simulation
puts "Starting Simulation..."
proc finish {} {
global ns_ tracefd namtrace
$ns_ flush-trace
close $tracefd
close $namtrace
exec nam 005.nam &
exec xgraph win5.tr &
exit 0
}
$ns_ run

}
$ns run
