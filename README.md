# na6

1. -i interface

Specify interface.

Example:
-i eth0

2. -s IPv6 source address

Used as the Source Address of the NA.

If blank → random link‑local unicast (fe80::/64)

During NDP spoofing, this often must match the expected NA source.

Example:
-s fe80::1

3. -d IPv6 destination address

Where the NA is sent.

If blank → ff02::1 (all-nodes multicast)

Passive mode: destination = source of incoming NS

If NS source = :: → use ff02::1

Example:
-d fe80::abcd

4. -A hop-limit

Default = 255 (required for ND).

Routers will not forward packets with hop‑limit 255 → stays in LAN.

Example:
-A 255

5. -y fragment

Forces IPv6 fragmentation.

⚠️ Real Neighbor Advertisements must NOT be fragmented, but na6 allows it for testing implementations.

Example:
-y 1280

6. -u Destination Options header

Adds a Destination Options extension header.

→ Processed only by the final receiver.
→ Not needed for normal NAs.

Example:
-u 20

7. -U Destination Options (unfragmentable part)

Same as -u, but placed in unfragmentable section of a fragmented packet.

Only valid if -y is used.

Example:
-U 16 -y 512

8. -H Hop-by-Hop header

Examined by every router along the path.
Not needed for NA.

Example:
-H 8

9. -S MAC source address

Ethernet only.

If blank → random MAC.

Passive mode:

If NS was to multicast → use -S (or random)

If NS was to unicast → copy NS packet’s destination MAC

Example:
-S aa:bb:cc:dd:ee:ff

10. -D MAC destination address

Ethernet only.

Default → all-nodes multicast: 33:33:00:00:00:01

Passive mode:

If NS source = :: → all-nodes multicast MAC

Else → NS sender MAC

Example:
-D 33:33:ff:12:34:56

11. -r router bit

Sets the R flag indicating “I am a router”.

Example:
-r

12. -c solicited bit

Sets S flag.

In passive mode, for unicast NS, S=1 automatically.

Example:
-c

13. -o override flag

Controls how the receiver updates its neighbor cache.

O=1: replace existing entry

O=0: use only if no entry exists

Example:
-o

14. -t target IPv6

The Target Address of the NA.

Example:
-t fe80::1234

If using -T, -t becomes a prefix:

Example:
-t 2001:db8:1::/64 -T 50

15. -E Target LLA Option (manual value)

Adds a TLLA option with the MAC you specify.

Example:
-E aa:bb:cc:dd:ee:ff

16. -e Target LLA Option (automatic)

Adds TLLA option automatically using the source MAC (-S).

Example:
-S 00:11:22:33:44:55 -e
TLLA = 00:11:22:33:44:55

17. -j block-src

Block incoming packets from specific IPv6 sources/prefix.

Example:
-j fe80::1
-j 2001:db8::/32

18. -k block-dst

Block incoming NS based on their IPv6 destination address.

Example:
-k fe80::abcd

19. -J block-link-src

Block packets based on source MAC.

Example:
-J aa:bb:cc:dd:ee:ff

20. -K block-link-dst

Block packets based on destination MAC.

Example:
-K 33:33:00:00:00:01

21. -b accept-src

Only accept packets from IPv6 source address/prefix.

Example:
-b fe80::1020

22. -g accept-dst

Only accept packets sent to specific IPv6 destination(s).

Example:
-g fe80::abcd

23. -B accept-link-src

Accept only NS that come from this MAC.

Example:
-B 00:11:22:33:44:55

24. -G accept-link-dst

Accept packets only if their destination MAC matches.

Example:
-G 33:33:00:00:00:01

25. -w block-target

Block NS based on their Target IPv6.

Example:
-w fe80::1000/120

26. -W accept-target

Only accept NS if Target matches IPv6/prefix.

Example:
-W fe80::abcd

27. -l loop

Send periodic NAs.

Example:
-l

28. -z sleep time

Time between looped NAs.

Example:
-l -z 2 → send every 2 seconds

29. -L passive mode

Listen for NS and respond automatically.

Cannot use with -l.

Example:
-L

30. -v / -vv verbose

-v = verbose
-vv = very verbose, shows which packets passed/failed filters

Example:
-vv

31. -T flood-targets

Send many NAs with different Target Addresses.

Default target range: fe80::/64
Unless -t prefix is specified.

Example:
-T 50
or
-t 2001:db8::/64 -T 100

32. -F flood-sources

Send many NAs with different Source Addresses.

Random from -s prefix

If no -s, random from fe80::/64

Example:
-F 20 -s fe80::abcd/64
