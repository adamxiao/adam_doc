qemu deliver data from tap to virtio
====================================

data flow
------------------------------------

.. graphviz::
   :alt: qemu deliver data flow

   digraph G {

     qemu_net_queue_deliver -> qemu_deliver_packet_iov [label="queue->deliver"];
     qemu_net_queue_deliver_iov -> qemu_deliver_packet_iov [label="queue->deliver"];

     qemu_deliver_packet_iov -> nc_sendv_compat;
     nc_sendv_compat -> virtio_net_receive [label="nc->info->receive"];
     qemu_deliver_packet_iov -> "nc->info->receive_iov";
     "nc->info->receive_iov" -> net_hub_port_receive_iov;
     net_hub_port_receive_iov -> qemu_net_queue_send_iov [label="..."];
     qemu_net_queue_send_iov -> qemu_net_queue_deliver_iov [label="send pkg"];
     
     virtio_net_receive, net_hub_port_receive_iov [shape=box];
     
     "tap.0" -> hub0port1 ->  hub0port0 -> "virtio-net-pci.0" -> eth0;

   }

