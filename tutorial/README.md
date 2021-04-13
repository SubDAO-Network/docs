# Setup SubDAO Node

## Build from source
```
git clone --recursive https://github.com/SubDAO-Network/subdao-node.git
cd subdao-node
cargo build
```

## Setup with Docker
Get the latest docker image from [here](https://drive.google.com/drive/folders/1VRm0puMeYOj6c8hHGNlKmZZyN9D8mo-v?usp=sharing)

Run

```
docker load < subdao-node-xxxxx.tar.gz
```

to load docker image.


Get `docker compose` from `https://github.com/SubDAO-Network/docs`

```
git clone https://github.com/SubDAO-Network/docs.git
```

To start these services, subdao-node, subdao-frontend, and polkadot apps. 

```
cd docs/docker
docker-compose up -d
```

Check status
```
cd docs/docker
docker-compose ps 
```

### Config

Open http://localhost:3001/ in browser, and add the below configration in `Setting -> Developer`

```
{
  "Address": "AccountId",
  "LookupSource": "AccountId"
}
```

# Setup Contracts
SubDAO Contracts are provided in `https://github.com/SubDAO-Network/subDAO-contracts`. It's developed with ink!.

## Get contracts

```
git clone --recursive https://github.com/SubDAO-Network/subDAO-contracts
```

## Use pre-compiled contracts
Pre-compiled contracts are provided in `subDAO-contracts/release` folder. Please use them if you just wanna try SubDAO.

## Compile contracts from source code

The SubDAO provides script to simplify the contract compilation process while collecting the editing results into a unified directory to facilitate contract deployment and usage. Execute in the project root directory

```bash
bash ./build.sh
```

All contract compilation results are saved in the release directory.

## Deploy

The SubDAO creates the substrate chain to connect the POLKADOT Ecology, and all contracts are deployed on the SubDAO chain. This section explains how to make use of Polkadot JS App to deploy contracts.


Download and compile Polkadot JS Apps code(v0.71.2), followed by yarn start startup, OR using provided docker image and compose file.  Access the front page http://localhost:3001/ and set the node IP and port.

![](./image-2.png)


### Upload contracts

Enter `Developer-> Contracts` and click Upload WASM.

![](./image-3.png)

Select the ABI and WASM files that required to deploy contract, click `Upload`, and `Submit and Sign`.

![](./image-4.png)

Wait a moment and the contract code will be uploaded.

### Deploy contracts

After you upload the contracts, you can instantiate the contract on the chain. In substrate, you need to perform the contract’s initialization function, usually new or the default function.

For SubDAO contracts, all contracts are instantiated by the main contract. The main contract is responsible for managing contract templates and DAO instantiations.

![](./image-5.png)


Select the initialization function call, fill in the initialization parameters, set the main contract administrator, and set the contract initial balance, click `Deploy` before set a proper endornment number, normally 500 is enough. Note that the deployment salt is used.


## Initialization

### Initialize main contract

The main contract manages the DAO templates and DAO instantiations. After the main contract is deployed, you need to initialize the template management function of Main, call the init function and set the code hash of the contract template manager.

![](./image-6.png)


### Add templates for DAO

For now, DAO templates can only be configured in the tool by calling the `addTemplate` function in the main contract, fill in the creator accountid and the code hash for each components in the template.

For example, we create a DAO template with vault management, DAO tokens, org management, voting, and also the basic information for the template.

After adding the DAO template, you can happily create the DAO  through the `SubDAO frond-end`. Jump to the frontend which is served at `http://localhost:3001/`.

![](./image-7.png)


### Creating DAO

After you create a template, you can create your own DAO from the template that you have set up.

# Setup SubDAO Front-end

## Install `Polkadot JS Extension`
Please install `Polkadot JS Extension` before you start. You can get it from here https://polkadot.js.org/extension/

### Get source code
Please get the code from `https://github.com/SubDAO-Network/subDAO-frontend`

```
git clone https://github.com/SubDAO-Network/subDAO-frontend.git
```

### Config front-end
Please find the correct address for `main_v0.1`, and update the correct address in `public/config.js`.

```
window.mainAddress = {
    main: "<MAIN CONTRACT ADDRESS>",
    rpc_server: "<SUBDAO NODE RPC>"
};
```

`<MAIN CONTRACT ADDRESS>` is the main contract's address after the contracts are deployed in pre steps.
`<SUBDAO NODE RPC>` is the websocket RPC provided by SubDAO Node. If you run it locally, it should be `ws://127.0.0.1:9944` by default.

### Install dependencies
Run `yarn` to install packages needed for this App.

### Start front-end
`yarn start` runs the app in the development mode.  
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

# Have fun!
Please refer [docs/usage](https://github.com/SubDAO-Network/docs/blob/main/usage/README.md) to create and manage your own DAO.
