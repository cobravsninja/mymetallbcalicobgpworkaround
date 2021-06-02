# My MetalLB/Calico BGP workaround for IPv4 24 bits subnets
1. Init container (I used my own alpine based image which contains only iproute2 package, due to lack of namespace support in busybox) mounts /proc/1/ns (from host) which contains default namespaces
2. Creates symlinks for both, container and default network namespaces
3. Creates macvtap device which points to eth0 (in host's default network namespace)
4. Moves the same device to container network namespace (from default host's network namespace)
5. Assigns IP addresses (${subnet}.24${last_digit_of_last_octet} if node's name contains \*node\*, ${subnet}.25${last_digit_of_last_octet} if node's name contains \*master\*)
6. There is limitation if nodes or masters count greater than 10, but who cares :-)
