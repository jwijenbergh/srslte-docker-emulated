# srsran-docker-emulated
This is a minimal example of a
[srsRAN](https://github.com/srsran/srsRAN) LTE system running with
Docker. The core network and base station run in separate
containers. This branch supports SDRs with the
[UHD](https://github.com/EttusResearch/uhd) driver. For the code
branch that supports a virtual UE (srsUE) using ZeroMQ, see:
[main](https://github.com/jwijenbergh/srsran-docker-emulated/tree/main).

## Configuration
Before you start, make sure to use the correct configs for your SDR
and phone setup. These configs can be applied by adding them to the
Dockerfile. For example, UEs have to be added to the [user db
file](https://github.com/srsran/srsRAN/blob/master/srsepc/user_db.csv.example)
to get internet access.
You can find the exact versions of all the config files in [srsepc], [srsenb] and [srsue].

[srsepc]: https://github.com/srsran/srsRAN/tree/5275f33360f1b3f1ee8d1c4d9ae951ac7c4ecd4e/srsepc
[srsenb]: https://github.com/srsran/srsRAN/tree/5275f33360f1b3f1ee8d1c4d9ae951ac7c4ecd4e/srsenb
[srsue]: https://github.com/srsran/srsRAN/tree/5275f33360f1b3f1ee8d1c4d9ae951ac7c4ecd4e/srsue

## Starting
- Start the containers with the following command:

```bash
docker-compose up
```

- When the UE has attached, enable routing for internet access by issuing the following command:
```bash
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward 1>/dev/null
sudo iptables -t nat -A POSTROUTING -o YOURNETWORKCARDHERE -j MASQUERADE
```

## Credits
* [srsRAN](https://github.com/srsran/srsRAN)
* [pgorczak](https://github.com/pgorczak): This container is a fork from https://github.com/pgorczak/srslte-docker-emulated
  - [jgiovatto](https://github.com/jgiovatto): Implemented shared memory interfaces in the previous version
  - [FabianEckermann](https://github.com/FabianEckermann): Integrating shared memory with docker IPC in the previous version
