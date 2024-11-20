VMNIC = carte réseau physique de l'ESXi, ne peut être branché que sur un seul vSwitch
vSwitch = Switch virtuel, fonctionne comme un switch de niveau 2 (ne peut pas router)
PortGroup = il existe deux types de PortGroup
- VMKernel attribue des IP et ne peuvent pas être connecté à des vNIC, il sert à rendre disponible des services sur le réseau. On ne peut pas mettre de VM sur ce PortGroup
- VM est relié à des VLANs et peut être connecté à des vNIC, fonctionne comme un VLAN.
vNIC = carte réseau d'une VM

Un vSwitch peut fonctionner sans VMNIC, ce qui signifie que les VMs reliés à ce vSwitch ne peuvent communiquer vers l'extérieur mais il peuvent communiquer avec les autres machines reliés sur ce vSwitch. Le fait de brancher une VMNIC à un vSwitch ne permet pas d'assurer le routage entre les PortGroup(VLAN). La VMNIC s'assure simplement que les VM connecté au vSwitch peuvent sortir de leur PortGroup.



dupliquer toute la configuration réseau entre les ESXi en cluster. Pour fonctionner 