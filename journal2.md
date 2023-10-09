
## Journal for the week of Oct. 2 - Oct. 8 2023
# Work on the "Dependable" Project
I committed my code for the NDP protocol to a [pull request](https://github.com/alain-lclark/dependable/pull/34). I fixed some problems such as memory allocation of queues and memory leaks.

I refactored memory allocation of queues by statically allocating queues with configurable size macro for each layer of the stack.

I fixed the memory leak issues that I was experiencing by debugging with vgdb, the valgrind server for gdb.
```c
sudo valgrind -q --vgdb-error=0 <name of test> # wait for gdb to attach before running any instructions
# in a separate pane
sudo gdb <name of test>
# inside gdb
target remote | /usr/libexec/valgrind/../../bin/vgdb --pid=<pid that valgrind spit out>
until <line number where exit is called in main>
monitor leak_check full reachable any # run a leak check and include still-reachable memory
monitor block_list 1 # output address of leaked memory in loss record 1
monitor who_points_at <address spit out by above command> # show variable holding address
# it turned out to be eth_rxq_buf, the buffer storing queued socket buffers
p *(struct sk_buf *)<leaked address>
# output:
$17 = {begin = 0x4a92e60, end = 0x4a93450, first = 0x4a92ec8, last = 0x4a92ed8, src = {
    ip_addr = {65152, 0, 0, 0, 5666, 15359, 65062, 43448}, port = 0}, dst = {ip_addr = {
      65282, 0, 0, 0, 0, 0, 0, 1}, port = 0}, ix = 0x4192c0 <eth>, next_hop = 0x0, 
  iphd = 0x4a92e70, upperhd = {udp = 0x4a92e98, icmp = 0x4a92e98}, ethd = 0x4a92e62, 
  proto = 58 ':', is_mcast = 0 '\000'}
# all of the leaks were proto = 58 (ICMP)
p icmp_hdr_ptr_get_type(<address of icmp_hdr in struct sk_buf>)
# the types were all 134 (router address)
```
I then went to the router address handler code and discovered that there was no free of the socket buffer in the case that the socket buffer was valid. I then added a free for this case, and a free for a similar case on another ICMP type that happened to be received less frequently.

# Work on Desktop Application

