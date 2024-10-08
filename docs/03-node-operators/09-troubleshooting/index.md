---
title: Troubleshooting
sidebar_label: Troubleshooting
sidebar_position: 11
tags: [rsk, rskj, rootstock, node, faq, troubleshoot]
description: "How to solve some known or frequently encountered issues when working with RSKj"
---

This section explains how to solve some known or frequently encountered issues.

If what you need is not in this section, **contact us** without hesitation through the [Rootstock Community on Discord](https://rootstock.io/discord). We will be happy to help you!

<Accordion>
  <Accordion.Item eventKey="0">
    <Accordion.Header as="h3">Discovery can't be started</Accordion.Header>
    <Accordion.Body>
        - On Windows, if you start the node and it doesn't do anything, there is a high chance you have a problem with the UDP port of the node.
        - The UDP port is configured in the node's configuration file, specifically with the value `peer.port`. By default this port is configured to `5050`.
        - To check if that port is already taken by other application you can follow these steps:
        - * Open a `cmd` console and run `netstat -ano -p UDP | findstr :5050` (or replace `5050` with the port of your preference).
        - * You will get a result with the process ID (if any) already using that port for UDP.
        - * With the process ID (the value at the far right), run this command `tasklist /FI "PID eq processId-you-got"`.
        - This will let you know which application/service is using this port.
        - Please make sure the port of your preference is not taken by other application. If so, you need to change the [node configuration](/node-operators/setup/configuration/preferences), by overwriting the [peer](/node-operators/setup/configuration/preferences).
    </Accordion.Body>
  </Accordion.Item>
  <Accordion.Item eventKey="1">
    <Accordion.Header as="h3">I don't see the logs</Accordion.Header>
    <Accordion.Body>
       - You can configure your own log level, following these [instructions](/node-operators/setup/configuration/verbosity).
    </Accordion.Body>
  </Accordion.Item>
  <Accordion.Item eventKey="2">
    <Accordion.Header as="h3">Plugin with id witness not found</Accordion.Header>
    <Accordion.Body>
        - If you have this error it's possible that you have missed to run rskj's dependencies.
        - So please, follow the instructions depending on your operation system:
        - [On Windows](/node-operators/setup/node-runner/windows)
        - [On Linux](/node-operators/setup/node-runner/linux)
        - [On Mac](/node-operators/setup/node-runner/macos)
    </Accordion.Body>
  </Accordion.Item>
  <Accordion.Item eventKey="3">
    <Accordion.Header as="h3">Truffle doesn't seem to work connected to Rootstock</Accordion.Header>
    <Accordion.Body>
       - If you can not get `truffle migrate` complete, you will see something like:
        ```javascript
            Writing artifacts to ./build/contracts
            Using network 'development'.
            Running migration: 1_initial_migration.js
            Deploying Migrations...
            0xc82d661d579e40d22c732b2162734f97aeb13fa095946927cbb8cd896b26a7a3
        ```
        - Be sure you are using the right configuration in the `truffle.js` and `truffle-config.js` files.
        - Remember that you need: **node host**, **node port**, **network_id** and in some cases the **from** (by default Truffle uses the first account in the node). This last one should be an account with positive balance (because it's the one Truffle uses to deploy contract and run transactions) and it should be present between the node's accounts (you can know that by executing the `web3.eth.accounts` command).
        - So, your config file should be like this:
        ``` javascript
        module.exports = {
        networks : {
            rsk: {
                from : "0xcd2a3d9f938e13cd947ec05abc7fe734df8dd826",
                host : "localhost",
                port : 4444,
                network_id : "*" // Match any network id
                }
            }
        };
        ```
    </Accordion.Body>
  </Accordion.Item>
  <Accordion.Item eventKey="4">
    <Accordion.Header as="h3">Can't get public IP</Accordion.Header>
    <Accordion.Body>
        - If you get the error:
        - `Can't get public IP` when you're trying to run your rskj node, the reason is that rskj uses Amazon Check IP service to set the [`public.ip`](/node-operators/setup/configuration/reference/) parameter.
        - To solve it, you need to change the `public.ip` key in config file with your IP address (if you don't know your IP, simply [search for it](https://www.google.com/search?q=what's+my+IP+address)).
        - Visit the [Config page](/node-operators/setup/configuration/) to change a node's configuration file.
    </Accordion.Body>
  </Accordion.Item>
  <Accordion.Item eventKey="5">
    <Accordion.Header as="h3">Rewind Blocks</Accordion.Header>
    <Accordion.Body>
        - This tool should be used in a scenario where an RSK node processes blocks that are corrupted or invalid, for example after a hard fork. It allows one to remove such blocks and start from a previously known state. It does so by removing the blocks with block number higher than the block number parameter command line argument.
        - Note: The node must be turned off before the rewind, and restarted after.
        - Example:
        `java -cp rsk-core-<VERSION>.jar co.rsk.cli.tools.RewindBlocks 1000000`
        - The above command removes the blocks with number 1000001 or higher.
    </Accordion.Body>
  </Accordion.Item>
  <Accordion.Item eventKey="6">
    <Accordion.Header as="h3">DbMigrate: Migrate between databases</Accordion.Header>
    <Accordion.Body>
       - This tool allows the user to migrate between different supported databases such as `rocksdb` and `leveldb`. 
        - How to use
           - To use the `DbMigrate` tool to migrate between databases, we will need a tool class and CLI arguments.
        - The tool class is: `co.rsk.cli.tools.DbMigrate`
        - Required CLI arguments:
            - `args[0]` - database target where we are going to insert the information from the current selected database.
            - Note: You cannot migrate to the same database or an error will be thrown. It is highly recommended to turn off the node in order to perform the migration since latest data could be lost.> > - Example migrating from `leveldb` to `rocksdb`:
            - `java -cp rsk-core-<VERSION>.jar co.rsk.cli.tools.DbMigrate rocksdb`
    </Accordion.Body>
  </Accordion.Item>
  <Accordion.Item eventKey="7">
    <Accordion.Header as="h3"> ERROR: failed to solve: failed to read dockerfile</Accordion.Header>
    <Accordion.Body>
        - The first error indicates that Docker couldn't find the `Dockerfile` in your current directory. Make sure you're in the correct directory or specify the path to the `Dockerfile`
        - If your Dockerfile is in txt, move the Dockerfile.txt to Dockerfile: `mv /path/to/Dockerfile.txt /path/to/Dockerfile`
        - Proceed with `Docker Build` command: `docker build -t regtest /path/to/rskj-node-jar`
    </Accordion.Body>
  </Accordion.Item>
  <Accordion.Item eventKey="8">
    <Accordion.Header as="h3">WARNING: The requested image's platform (linux/amd64) does not match</Accordion.Header>
    <Accordion.Body>
       - This warning indicates that the platform of the image doesn't match the platform of your host machine. The image is built for linux/amd64 architecture, but your host machine is linux/arm64/v8 architecture.
        - Use a compatible image: `docker run -d --name rsk-node -p 4444:4444 -p 50505:50505 rsksmart/rskj:arm64v8-latest node --regtest`
    </Accordion.Body>
  </Accordion.Item>
</Accordion>


