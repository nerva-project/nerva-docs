# Nerva Docker Guide
This explains how to run the Nerva daemon and the CLI wallet in Docker containers, using the official image published on Docker Hub. It is aimed at anyone who prefers to keep node software containerized: a home server, a small VPS, a Raspberry Pi. Nothing here is particularly Nerva specific beyond the image name, the ports and the data locations, so if you already run other coin daemons in Docker this should feel familiar.

The image is built from the same source tree as the official release binaries and is published for both amd64 and arm64, so the same tag runs on an x86 server and on an ARM board. Version tags follow the release version, for example `v0.3.0.0`, and `latest` tracks the most recent release.

<hr>

# Running the daemon

Pull the image. Pinning a version tag instead of `latest` is a good habit with blockchain software, because you want to be the one deciding when your node moves across a hard fork:

    docker pull sn1f3rt/nerva:v0.3.0.0

If the wallet will run in its own container, put both containers on the same Docker network so they can reach each other by name:

    docker network create --driver bridge nerva

Then start the daemon:

    docker run -d --rm --name nerva-daemon \
    --net=nerva \
    -v daemon:/data \
    -p 17565:17565 \
    -p 17566:17566 \
    sn1f3rt/nerva:v0.3.0.0 \
    nervad \
    --data-dir=/data \
    --rpc-bind-ip=0.0.0.0 \
    --confirm-external-bind \
    --non-interactive

A few details worth understanding. The named volume `daemon` holds the blockchain database, so it survives the container being removed and recreated. Port 17565 carries the peer to peer traffic and 17566 the RPC interface; publishing them is only needed if something outside Docker talks to the node. `--confirm-external-bind` acknowledges that the RPC will accept connections that do not come from localhost, which is the case for the wallet container. `--non-interactive` disables the interactive console, which has no meaning in a detached container.

One thing to watch out for: anything you write after the image name replaces the default command baked into the image, and the default command is what binds the RPC to all interfaces. If you pass a custom `nervad` line like the one above, keep `--rpc-bind-ip=0.0.0.0` in it. Without it the daemon listens on loopback inside the container only, and neither the published port nor the wallet container will be able to reach it.

Syncing progress can be followed with `docker logs -f nerva-daemon`. Expect the familiar `SYNCHRONIZED OK` message once the chain is up to date, like with any other node.

# Running the wallet

The wallet can run in a second container against the daemon above:

    docker run --rm -it --name nerva-wallet \
    --net=nerva \
    -v wallet:/wallet \
    -w /wallet \
    sn1f3rt/nerva:v0.3.0.0 \
    nerva-wallet-cli \
    --trusted-daemon \
    --daemon-address nerva-daemon:17566

The `-w /wallet` part sets the working directory to `/wallet`, which the image provides as a volume owned by the `nerva` user, so your wallet files land in the persistent `wallet` volume instead of inside the container filesystem. Since the wallet talks to the daemon container over the Docker network, the daemon is technically remote, and it is told so with `--daemon-address`. It is a node you run yourself, so `--trusted-daemon` is appropriate.

If you prefer to keep everything in one container, you can also open a shell inside the running daemon container and create the wallet next to it:

    docker exec -it nerva-daemon bash
    cd /wallet
    nerva-wallet-cli --trusted-daemon

From inside the daemon container, the wallet talks to the daemon over loopback, which is trusted by default.

Whichever way you run it, the usual wallet rules apply: write your 25 word mnemonic seed down, keep the password safe, and remember that the seed is the only thing that can bring the wallet back if the volume is lost.

<hr>

# Day to day operation

Stopping the node is `docker stop nerva-daemon`. The daemon handles the shutdown signal and closes the database cleanly, so a plain stop is fine and there is no need to reach for `docker kill`.

Upgrading to a new release means pulling the new tag and recreating the container with the same options and the same volume:

    docker pull sn1f3rt/nerva:v0.3.0.1
    docker rm -f nerva-daemon
    docker run -d ... sn1f3rt/nerva:v0.3.0.1 nervad --data-dir=/data --rpc-bind-ip=0.0.0.0 --confirm-external-bind --non-interactive

The blockchain database stays in the `daemon` volume and the new container picks up where the old one left off. Around a scheduled hard fork this is the whole upgrade procedure; the daemon will refuse to mine past the fork height on old software, but syncing an already upgraded chain keeps working for a while. As always with hard forks, it is better to upgrade a few days early than a few days late.

If you want the container to come back after a reboot, either write a small systemd unit or use Docker's restart policy, for example by adding `--restart unless-stopped` to the daemon line. Both approaches work equally well; the restart policy is the quicker one to set up.

# Building the image yourself

The [source repository][nerva-github-link] contains the `Dockerfile` at its root, so a local image can be built with:

    git clone --recursive https://github.com/nerva-project/nerva.git
    cd nerva
    docker build -t nerva:local .

The build is a multistage one: a builder stage compiles the release binaries through the depends system, and a runtime stage copies them into a clean Ubuntu image. It needs Docker 17.05 or newer, and the compilation is heavy, so expect the first build to take a while and to eat a few gigabytes of disk. The build picks up the architecture of the machine it runs on, and the official multi-arch images are produced the same way with buildx.

Running an image you built yourself is the most self-contained option, since you know exactly what went into it. For most people though, pulling the published image and verifying behavior against the release notes is the practical middle ground.

<!--Reference links -->
[nerva-github-link]: https://github.com/nerva-project/nerva
