# 1. Create and deploy a simple, smart contract with cargo casper and cargo test

**Compile and test contract:**
```
cargo install cargo-casper
cargo casper simple-project
cd simple-project
rustup install $(cat rust-toolchain)
rustup target add --toolchain $(cat rust-toolchain) wasm32-unknown-unknown
cd contract
cargo build --release
cd ..
make test
```
![1](https://user-images.githubusercontent.com/21956707/134070332-4f4ba5c9-b237-4237-8464-f19ec214c2c9.png)
![2](https://user-images.githubusercontent.com/21956707/134070344-cdee24c8-d21b-4a48-908d-230000c2b8ff.png)
![3](https://user-images.githubusercontent.com/21956707/134070350-a2dcbef2-ff04-4d92-b094-17a91281bfb5.png)
![4](https://user-images.githubusercontent.com/21956707/134070358-c253a081-6f3c-416f-bf9a-f943a20ff88a.png)

**Installing the Client:**
```
rustup toolchain install nightly
cargo +nightly-2021-06-17 install casper-client --locked
```
![5](https://user-images.githubusercontent.com/21956707/134071017-a5da198c-eec0-4bfd-a2f3-ef6b54843a1d.png)

**Generated keys:**

```
casper-client keygen ~/.casper-keys
```
![6](https://user-images.githubusercontent.com/21956707/134071670-edd2c809-f09e-450f-9a96-ecd55652f09f.png)

I requested token from faucet (https://testnet.cspr.live/tools/faucet) and now it's time to deploy:
```
casper-client put-deploy --chain-name casper-test --node-address http://157.230.49.238:7777 --secret-key ~/.casper-keys/secret_key.pem --session-path ~/simple-project/contract/target/wasm32-unknown-unknown/release/contract.wasm  --payment-amount 10000000
```
![7](https://user-images.githubusercontent.com/21956707/134121334-d1328c96-b411-4e4a-b820-7a1b016d2233.png)

Checking deployment status:
```
casper-client get-deploy --node-address http://157.230.49.238:7777 6bb6502ff14ca08b38eca7754d5fa95fc275b1ff9f42c5bb607e01ecc0023db9
```
```
{
  "id": 2909234076284579023,
  "jsonrpc": "2.0",
  "result": {
    "api_version": "1.3.2",
    "deploy": {
      "approvals": [
        {
          "signature": "[130 hex chars]",
          "signer": "01e456c3779510fd14e83fa3be84ff4b2a22de76ef6be677ed7936f37f7712a0a4"
        }
      ],
      "hash": "6bb6502ff14ca08b38eca7754d5fa95fc275b1ff9f42c5bb607e01ecc0023db9",
      "header": {
        "account": "01e456c3779510fd14e83fa3be84ff4b2a22de76ef6be677ed7936f37f7712a0a4",
        "body_hash": "47de396bf9d2171ffda855bae540dfc7070dca083f835caf1859bb99e687635f",
        "chain_name": "casper-test",
        "dependencies": [],
        "gas_price": 1,
        "timestamp": "2021-09-21T06:19:15.382Z",
        "ttl": "30m"
      },
      "payment": {
        "ModuleBytes": {
          "args": [
            [
              "amount",
              {
                "bytes": "03809698",
                "cl_type": "U512",
                "parsed": "10000000"
              }
            ]
          ],
          "module_bytes": ""
        }
      },
      "session": {
        "ModuleBytes": {
          "args": [],
          "module_bytes": "[51990 hex chars]"
        }
      }
    },
    "execution_results": [
      {
        "block_hash": "a0b1ccfdb607b1d84143c4cdba8791e504c413f2af51eb7d17d802106a740fb7",
        "result": {
          "Failure": {
            "cost": "1383320",
            "effect": {
              "operations": [
                {
                  "key": "hash-9824d60dc3a5c44a20b9fd260a412437933835b52fc683d8ae36e4ec2114843e",
                  "kind": "Read"
                },
                {
                  "key": "balance-904bf9e4268486082d950c06ffcb5ff684b3d4547d8199a819248d1ab3f18c86",
                  "kind": "Write"
                },
                {
                  "key": "balance-98d945f5324f865243b7c02c0417ab6eac361c5c56602fd42ced834a1ba201b6",
                  "kind": "Read"
                },
                {
                  "key": "hash-010c3fe81b7b862e50c77ef9a958a05bfa98444f26f96f23d37a13c96244cfb7",
                  "kind": "Read"
                },
                {
                  "key": "hash-8cf5e4acf51f54eb59291599187838dc3bc234089c46fc6ca8ad17e762ae4401",
                  "kind": "Read"
                },
                {
                  "key": "balance-53ecfeb03aed32aa49093ea7d3ce5123e1625e84897ea5b25abe1d29118ef2e9",
                  "kind": "Write"
                },
                {
                  "key": "hash-624dbe2395b9d9503fbee82162f1714ebff6b639f96d2084d26d944c354ec4c5",
                  "kind": "Read"
                }
              ],
              "transforms": [
                {
                  "key": "hash-624dbe2395b9d9503fbee82162f1714ebff6b639f96d2084d26d944c354ec4c5",
                  "transform": "Identity"
                },
                {
                  "key": "hash-010c3fe81b7b862e50c77ef9a958a05bfa98444f26f96f23d37a13c96244cfb7",
                  "transform": "Identity"
                },
                {
                  "key": "balance-904bf9e4268486082d950c06ffcb5ff684b3d4547d8199a819248d1ab3f18c86",
                  "transform": {
                    "WriteCLValue": {
                      "bytes": "0580790cd4e8",
                      "cl_type": "U512",
                      "parsed": "999990000000"
                    }
                  }
                },
                {
                  "key": "hash-8cf5e4acf51f54eb59291599187838dc3bc234089c46fc6ca8ad17e762ae4401",
                  "transform": "Identity"
                },
                {
                  "key": "balance-98d945f5324f865243b7c02c0417ab6eac361c5c56602fd42ced834a1ba201b6",
                  "transform": "Identity"
                },
                {
                  "key": "hash-9824d60dc3a5c44a20b9fd260a412437933835b52fc683d8ae36e4ec2114843e",
                  "transform": "Identity"
                },
                {
                  "key": "balance-53ecfeb03aed32aa49093ea7d3ce5123e1625e84897ea5b25abe1d29118ef2e9",
                  "transform": {
                    "AddUInt512": "10000000"
                  }
                }
              ]
            },
            "error_message": "ApiError::MissingArgument [2]",
            "transfers": []
          }
        }
      }
    ]
  }
}
```

# 2. A Counter Contract Tutorial

First I completed NCTL tutorial (https://docs.casperlabs.io/en/latest/dapp-dev-guide/setup-nctl.html).

I cloned counter repo:
`git clone https://github.com/casper-ecosystem/counter`
and started local network:
`nctl-assets-setup && nctl-start`

![8](https://user-images.githubusercontent.com/21956707/134144978-f1761564-fec5-4ead-9e80-ec6264354d1b.png)

Next I got faucet account information which I will need in later steps:
`nctl-view-faucet-account`
```
{
  "api_version": "1.0.0",
  "merkle_proof": "[2092 hex chars]",
  "stored_value": {
    "Account": {
      "account_hash": "account-hash-3ff899ca5992454a83050876e1419632179a824b4c98f71a96a1684d946944a2",
      "action_thresholds": {
        "deployment": 1,
        "key_management": 1
      },
      "associated_keys": [
        {
          "account_hash": "account-hash-3ff899ca5992454a83050876e1419632179a824b4c98f71a96a1684d946944a2",
          "weight": 1
        }
      ],
      "main_purse": "uref-7d1d1b9188505616fc7199bb62eab17368e69d1b2b0bb89032cb39af9dbcee81-007",
      "named_keys": []
    }
  }
}
```

Get state root hash:
`casper-client get-state-root-hash --node-address http://localhost:11101`
![9](https://user-images.githubusercontent.com/21956707/134145845-9e8cfec9-28d9-44ef-8c83-179a9ca4941f.png)

Then we query the state:
```
casper-client query-state --node-address http://localhost:11101 --state-root-hash 8fe56bd6150f4e8562f941f66ff6d3c0b728191a6d2583efba365c07b4603b47 --key 013c78e3b26a7a8dbdd621d92d15957068b12cac141d1c5048ed84087ab5404333
```
![10](https://user-images.githubusercontent.com/21956707/134146357-bdc48756-d520-42f1-9426-66d70e5184c9.png)

Deploy the Counter smart contract:
```
make prepare
make test
casper-client put-deploy \
    --node-address http://localhost:11101 \
    --chain-name casper-net-1 \
    --secret-key /home/venoox/casper/casper-node/utils/nctl/assets/net-1/faucet/secret_key.pem \
    --payment-amount 5000000000000 \
    --session-path ./target/wasm32-unknown-unknown/release/counter-define.wasm
```
![11](https://user-images.githubusercontent.com/21956707/134146856-491a05ad-4e6e-43ea-bc8e-da0a1a6100b8.png)
```
{
  "id": 5314631676092110967,
  "jsonrpc": "2.0",
  "result": {
    "api_version": "1.0.0",
    "deploy_hash": "d8dce070bb792e5284dda6c31fd2126528ef67e130f4381e0b6053d5f30edce8"
  }
}
```

Let's view the network state again:
```
casper-client get-state-root-hash --node-address http://localhost:11101
casper-client query-state \
    --node-address http://localhost:11101 \
    --state-root-hash 4fbcf292df0e14ad71365f6fc4377254927823db4151eeb882cf5df9de3c98b2 \
    --key 013c78e3b26a7a8dbdd621d92d15957068b12cac141d1c5048ed84087ab5404333
```
![12](https://user-images.githubusercontent.com/21956707/134147339-3b59ed3c-930f-4fc7-be57-4e86955bcc10.png)

We can also look at specific details:
```
casper-client query-state \
    --node-address http://localhost:11101 \
    --state-root-hash 4fbcf292df0e14ad71365f6fc4377254927823db4151eeb882cf5df9de3c98b2 \
    --key 013c78e3b26a7a8dbdd621d92d15957068b12cac141d1c5048ed84087ab5404333 \
    -q "counter"
```
```
{
  "id": -1795756429307724746,
  "jsonrpc": "2.0",
  "result": {
    "api_version": "1.0.0",
    "merkle_proof": "[4178 hex chars]",
    "stored_value": {
      "Contract": {
        "contract_package_hash": "contract-package-wasma10238caeb05dbf758f292874bad6cc372ce22ec042ac4bd68109f4899f35940",
        "contract_wasm_hash": "contract-wasm-b0c6be132a3a1154fdc21d72c6b6bde5615399e9db907373a5f21cb5ba7b7927",
        "entry_points": [
          {
            "access": "Public",
            "args": [],
            "entry_point_type": "Contract",
            "name": "counter_get",
            "ret": "I32"
          },
          {
            "access": "Public",
            "args": [],
            "entry_point_type": "Contract",
            "name": "counter_inc",
            "ret": "Unit"
          }
        ],
        "named_keys": [
          {
            "key": "uref-db1c55da0b95e2b8c2fc0c6fef59966bccbeb99c135fa9ca9bd91c7b7f8850d0-007",
            "name": "count"
          }
        ],
        "protocol_version": "1.0.0"
      }
    }
  }
}
```

```
casper-client query-state \
    --node-address http://localhost:11101 \
    --state-root-hash 4fbcf292df0e14ad71365f6fc4377254927823db4151eeb882cf5df9de3c98b2 \
    --key 013c78e3b26a7a8dbdd621d92d15957068b12cac141d1c5048ed84087ab5404333 \
    -q "counter/count"
```
```
{
  "id": 8618847728453110348,
  "jsonrpc": "2.0",
  "result": {
    "api_version": "1.0.0",
    "merkle_proof": "[7970 hex chars]",
    "stored_value": {
      "CLValue": {
        "bytes": "00000000",
        "cl_type": "I32",
        "parsed": 0
      }
    }
  }
}
```

Next up we will make a call and increase the counter:
```
casper-client put-deploy \
    --node-address http://localhost:11101 \
    --chain-name casper-net-1 \
    --secret-key /home/venoox/casper/casper-node/utils/nctl/assets/net-1/faucet/secret_key.pem \
    --payment-amount 5000000000000 \
    --session-name "counter" \
    --session-entry-point "counter_inc"
```
![13](https://user-images.githubusercontent.com/21956707/134147721-8f4f92c6-e116-4f37-af0a-78096b5910aa.png)

And we look at updated state:
```
casper-client get-state-root-hash --node-address http://localhost:11101
casper-client query-state --node-address http://localhost:11101 \
    --state-root-hash cce851fd6f752a3b0a3a7af073d09810f6fbd0406bf298807aa2126fd46fe431 \
    --key 013c78e3b26a7a8dbdd621d92d15957068b12cac141d1c5048ed84087ab5404333 -q "counter/count"
```
```
{
  "id": -5368024686293240179,
  "jsonrpc": "2.0",
  "result": {
    "api_version": "1.0.0",
    "merkle_proof": "[7970 hex chars]",
    "stored_value": {
      "CLValue": {
        "bytes": "01000000",
        "cl_type": "I32",
        "parsed": 1
      }
    }
  }
}
```

For the last part we will deploy a contract that will increase the counter of already deployed Counter:
```
casper-client put-deploy \
    --node-address http://localhost:11101 \
    --chain-name casper-net-1 \
    --secret-key /home/venoox/casper/casper-node/utils/nctl/assets/net-1/faucet/secret_key.pem \
    --payment-amount 5000000000000 \
    --session-path ./target/wasm32-unknown-unknown/release/counter-call.wasm
```
![14](https://user-images.githubusercontent.com/21956707/134148258-475b62ad-c67c-41b5-9fdd-898f90268d5e.png)

And we should see the number increase to 2:
```
casper-client get-state-root-hash --node-address http://localhost:11101
casper-client query-state --node-address http://localhost:11101 \
    --state-root-hash 29b044ced4c89515c0fdff7553b05bf186390583a12a57861ef92ee26fce4975 \
    --key 3f833226f4af3323c2c0934190f680e7506604355d01287d252af90d84e1e077 -q "counter/count"
```
![15](https://user-images.githubusercontent.com/21956707/134149113-b7e31a2e-1172-4725-9b3f-b761008a5a5f.png)


# 3. Key management and implementing Scenario 3

First I cloned keys-manager: `git clone https://github.com/casper-ecosystem/keys-manager`
and build it: `cargo build --release`

The modify client's env file to:
```
BASE_KEY_PATH=/home/venoox/casper/casper-node/utils/nctl/assets/net-1/faucet/
NODE_URL=http://localhost:11101/rpc
```
I copied scenario-all.js and modified to fit scenario 3:

```js
const keyManager = require('./key-manager');
const TRANSFER_AMOUNT = process.env.TRANSFER_AMOUNT || 2500000000;

(async function () {

    // Scenario 3
    
    // To achive the task, we will:
    // 1. Set Keys Management Threshold to 2.
    // 2. Set Deploy Threshold to 1.
    // 3. Set both accounts to 1.
    // 4. Add a new key using mainAccount and firstAccount.
    // 5. Remove newly added key using mainAccount and firstAccount.

    const masterKey = keyManager.randomMasterKey();
    const mainAccount = masterKey.deriveIndex(1);
    const firstAccount = masterKey.deriveIndex(2);
    const secondAccount = masterKey.deriveIndex(3);

    console.log("Main account: " + mainAccount.publicKey.toHex());
    console.log("First account: " + firstAccount.publicKey.toHex());

    console.log("\n[x] Funding main account.");
    await keyManager.fundAccount(mainAccount);
    await keyManager.printAccount(mainAccount);

    console.log("\n[x] Install Keys Manager contract");
    let deploy = keyManager.keys.buildContractInstallDeploy(mainAccount);
    await keyManager.sendDeploy(deploy, [mainAccount]);
    await keyManager.printAccount(mainAccount);

    // Deploy threshold is 1
    const deployThereshold = 1;
    // Key Managment threshold is 2
    const keyManagementThreshold = 2;

    const accounts = [
        { publicKey: mainAccount.publicKey, weight: 1 },
        { publicKey: firstAccount.publicKey, weight: 1 },
    ];

    console.log("\n[x] Update keys deploy.");
    deploy = keyManager.keys.setAll(mainAccount, deployThereshold, keyManagementThreshold, accounts);
    await keyManager.sendDeploy(deploy, [mainAccount]);
    await keyManager.printAccount(mainAccount);

    console.log("\n[x] Add a new key with weight 1.\n");
    deploy = keyManager.keys.setKeyWeightDeploy(mainAccount, secondAccount, 1);
    await keyManager.sendDeploy(deploy, [mainAccount, firstAccount]);
    await keyManager.printAccount(mainAccount);

    console.log("\n[x] Remove a newly added key.\n");
    deploy = keyManager.keys.setKeyWeightDeploy(mainAccount, secondAccount, 0);
    await keyManager.sendDeploy(deploy, [mainAccount, firstAccount]);
    await keyManager.printAccount(mainAccount);

})();
```

Console output:
```
[venoox@venoox-pc client]$ npm run start:three

> keys-manager@1.0.0 start:three
> node -r dotenv/config ./src/scenario-3.js

Main account: 0202bd02b9af3994ecb8a8a22c970e647b1575093be14b46866d19803c451ca02322
First account: 0202a2b6a2e27783f2be219599749ceed32ad69bd35166c89586b7b229e96790ded2

[x] Funding main account.
Signed by: account-hash-3ff899ca5992454a83050876e1419632179a824b4c98f71a96a1684d946944a2
Deploy hash: 915d829211b476fa6f5dcd953232c2f5b2cc43ccff3d20e744ccb11d35890793
Deploy result:
{
  deploy: {
    hash: '915d829211b476fa6f5dcd953232c2f5b2cc43ccff3d20e744ccb11d35890793',
    header: {
      account: '013c78e3b26a7a8dbdd621d92d15957068b12cac141d1c5048ed84087ab5404333',
      timestamp: '2021-09-21T13:37:43.188Z',
      ttl: '30m',
      gas_price: 1,
      body_hash: '32048456c13834161104e77754508a8785ccd922842b946c5a470a53827db082',
      dependencies: [],
      chain_name: 'casper-net-1'
    },
    payment: { ModuleBytes: [Object] },
    session: { Transfer: [Object] },
    approvals: [ [Object] ]
  }
}

[x] Current state of the account:
{
  _accountHash: 'account-hash-0156c023445ae7953cb1af84505aeed32048c48477626c051f2f3a66378e7652',
  namedKeys: [],
  mainPurse: 'uref-189b85eba2db16ed784da6e46b6dce6729008d5c8feceec26a69c83c71151e9f-007',
  associatedKeys: [
    {
      accountHash: 'account-hash-0156c023445ae7953cb1af84505aeed32048c48477626c051f2f3a66378e7652',
      weight: 1
    }
  ],
  actionThresholds: { deployment: 1, keyManagement: 1 }
}

[x] Install Keys Manager contract
Signed by: account-hash-0156c023445ae7953cb1af84505aeed32048c48477626c051f2f3a66378e7652
Deploy hash: a7ab75039441ac58d48423f8eebabae95f5121bbbd6b541a4976905c67fb0cf7
Deploy result:
{
  deploy: {
    hash: 'a7ab75039441ac58d48423f8eebabae95f5121bbbd6b541a4976905c67fb0cf7',
    header: {
      account: '0202bd02b9af3994ecb8a8a22c970e647b1575093be14b46866d19803c451ca02322',
      timestamp: '2021-09-21T13:38:49.664Z',
      ttl: '30m',
      gas_price: 1,
      body_hash: 'c348c29f842d188fdcd24766b1bb3fbb017478bfa792590a688c63c72303c5b7',
      dependencies: [],
      chain_name: 'casper-net-1'
    },
    payment: { ModuleBytes: [Object] },
    session: { ModuleBytes: [Object] },
    approvals: [ [Object] ]
  }
}

[x] Current state of the account:
{
  _accountHash: 'account-hash-0156c023445ae7953cb1af84505aeed32048c48477626c051f2f3a66378e7652',
  namedKeys: [
    {
      name: 'keys_manager',
      key: 'hash-6b62145ee181ffbd871c9fcd96098be14a026fa316e55aca44efbfe01f550cd9'
    },
    {
      name: 'keys_manager_hash',
      key: 'uref-0cbe70f0eabb43c27d6fd29501bcd53976a9fdb2c3b13e6712fe2e2a804fd686-007'
    }
  ],
  mainPurse: 'uref-189b85eba2db16ed784da6e46b6dce6729008d5c8feceec26a69c83c71151e9f-007',
  associatedKeys: [
    {
      accountHash: 'account-hash-0156c023445ae7953cb1af84505aeed32048c48477626c051f2f3a66378e7652',
      weight: 1
    }
  ],
  actionThresholds: { deployment: 1, keyManagement: 1 }
}

[x] Update keys deploy.
Signed by: account-hash-0156c023445ae7953cb1af84505aeed32048c48477626c051f2f3a66378e7652
Deploy hash: 318782ee53fe69abafb2c50e3541962e2e3cd24bfba90c7d41ad15e931de6c11
Deploy result:
{
  deploy: {
    hash: '318782ee53fe69abafb2c50e3541962e2e3cd24bfba90c7d41ad15e931de6c11',
    header: {
      account: '0202bd02b9af3994ecb8a8a22c970e647b1575093be14b46866d19803c451ca02322',
      timestamp: '2021-09-21T13:39:55.009Z',
      ttl: '30m',
      gas_price: 1,
      body_hash: '5e0f62f527f3785003762fa540f99bc403a3d0ac3f02fb9902867f1434de6d3a',
      dependencies: [],
      chain_name: 'casper-net-1'
    },
    payment: { ModuleBytes: [Object] },
    session: { StoredContractByName: [Object] },
    approvals: [ [Object] ]
  }
}

[x] Current state of the account:
{
  _accountHash: 'account-hash-0156c023445ae7953cb1af84505aeed32048c48477626c051f2f3a66378e7652',
  namedKeys: [
    {
      name: 'keys_manager',
      key: 'hash-6b62145ee181ffbd871c9fcd96098be14a026fa316e55aca44efbfe01f550cd9'
    },
    {
      name: 'keys_manager_hash',
      key: 'uref-0cbe70f0eabb43c27d6fd29501bcd53976a9fdb2c3b13e6712fe2e2a804fd686-007'
    }
  ],
  mainPurse: 'uref-189b85eba2db16ed784da6e46b6dce6729008d5c8feceec26a69c83c71151e9f-007',
  associatedKeys: [
    {
      accountHash: 'account-hash-0156c023445ae7953cb1af84505aeed32048c48477626c051f2f3a66378e7652',
      weight: 1
    },
    {
      accountHash: 'account-hash-2eac8a900a6ad91927a290a0155aee13529862b4f08289ea5245a9467ec64f67',
      weight: 1
    }
  ],
  actionThresholds: { deployment: 1, keyManagement: 2 }
}

[x] Add a new key with weight 1.

Signed by: account-hash-0156c023445ae7953cb1af84505aeed32048c48477626c051f2f3a66378e7652
Signed by: account-hash-2eac8a900a6ad91927a290a0155aee13529862b4f08289ea5245a9467ec64f67
Deploy hash: fd493b16397fecd4ff2cf79cd53151c1e3393f6f16334239daf09872fbeb5b9d
Deploy result:
{
  deploy: {
    hash: 'fd493b16397fecd4ff2cf79cd53151c1e3393f6f16334239daf09872fbeb5b9d',
    header: {
      account: '0202bd02b9af3994ecb8a8a22c970e647b1575093be14b46866d19803c451ca02322',
      timestamp: '2021-09-21T13:41:00.600Z',
      ttl: '30m',
      gas_price: 1,
      body_hash: '279cd0865be0838000e240c95d481bede0b0286f0ea9776a45542beeb3a2eddf',
      dependencies: [],
      chain_name: 'casper-net-1'
    },
    payment: { ModuleBytes: [Object] },
    session: { StoredContractByName: [Object] },
    approvals: [ [Object], [Object] ]
  }
}

[x] Current state of the account:
{
  _accountHash: 'account-hash-0156c023445ae7953cb1af84505aeed32048c48477626c051f2f3a66378e7652',
  namedKeys: [
    {
      name: 'keys_manager',
      key: 'hash-6b62145ee181ffbd871c9fcd96098be14a026fa316e55aca44efbfe01f550cd9'
    },
    {
      name: 'keys_manager_hash',
      key: 'uref-0cbe70f0eabb43c27d6fd29501bcd53976a9fdb2c3b13e6712fe2e2a804fd686-007'
    }
  ],
  mainPurse: 'uref-189b85eba2db16ed784da6e46b6dce6729008d5c8feceec26a69c83c71151e9f-007',
  associatedKeys: [
    {
      accountHash: 'account-hash-0156c023445ae7953cb1af84505aeed32048c48477626c051f2f3a66378e7652',
      weight: 1
    },
    {
      accountHash: 'account-hash-2eac8a900a6ad91927a290a0155aee13529862b4f08289ea5245a9467ec64f67',
      weight: 1
    },
    {
      accountHash: 'account-hash-d3433951324df5b29e2f8a68e7513ccf9f2badace9d33dc32b814fe768865ead',
      weight: 1
    }
  ],
  actionThresholds: { deployment: 1, keyManagement: 2 }
}

[x] Remove a newly added key.

Signed by: account-hash-0156c023445ae7953cb1af84505aeed32048c48477626c051f2f3a66378e7652
Signed by: account-hash-2eac8a900a6ad91927a290a0155aee13529862b4f08289ea5245a9467ec64f67
Deploy hash: 3db5921bc2f8e0fe3deec896234f9ae17d75d7b6c4142015bdef28b2f1f11620
Deploy result:
{
  deploy: {
    hash: '3db5921bc2f8e0fe3deec896234f9ae17d75d7b6c4142015bdef28b2f1f11620',
    header: {
      account: '0202bd02b9af3994ecb8a8a22c970e647b1575093be14b46866d19803c451ca02322',
      timestamp: '2021-09-21T13:42:06.881Z',
      ttl: '30m',
      gas_price: 1,
      body_hash: 'a265fdf14dbd12873193564adb390a63e7a0a5b5d3b051013e926dfa61c21e48',
      dependencies: [],
      chain_name: 'casper-net-1'
    },
    payment: { ModuleBytes: [Object] },
    session: { StoredContractByName: [Object] },
    approvals: [ [Object], [Object] ]
  }
}

[x] Current state of the account:
{
  _accountHash: 'account-hash-0156c023445ae7953cb1af84505aeed32048c48477626c051f2f3a66378e7652',
  namedKeys: [
    {
      name: 'keys_manager',
      key: 'hash-6b62145ee181ffbd871c9fcd96098be14a026fa316e55aca44efbfe01f550cd9'
    },
    {
      name: 'keys_manager_hash',
      key: 'uref-0cbe70f0eabb43c27d6fd29501bcd53976a9fdb2c3b13e6712fe2e2a804fd686-007'
    }
  ],
  mainPurse: 'uref-189b85eba2db16ed784da6e46b6dce6729008d5c8feceec26a69c83c71151e9f-007',
  associatedKeys: [
    {
      accountHash: 'account-hash-0156c023445ae7953cb1af84505aeed32048c48477626c051f2f3a66378e7652',
      weight: 1
    },
    {
      accountHash: 'account-hash-2eac8a900a6ad91927a290a0155aee13529862b4f08289ea5245a9467ec64f67',
      weight: 1
    }
  ],
  actionThresholds: { deployment: 1, keyManagement: 2 }
}
```

# 4. Transfer tokens to an account on the Casper Testnet.

```
casper-client transfer \
    --id 1 \
    --transfer-id 123456789012345 \
    --node-address http://157.230.49.238:7777 \
    --amount 2500000000 \
    --payment-amount 15000 \
    --secret-key ~/.casper-keys/secret_key.pem \
    --chain-name casper-test \
    --target-account 0172a54c123b336fb1d386bbdff450623d1b5da904f5e2523b3e347b6d7573ae80
```
![16](https://user-images.githubusercontent.com/21956707/134186317-00b30421-c876-4516-b5fb-56cfa762cbdf.png)

Deployed hash: `9ea2fec7b41a829638c8917bc3fe7134a76469596c6a42561b26182e6f484973`

Query deploy:
```
casper-client get-deploy \
      --id 2 \
      --node-address http://157.230.49.238:7777 \
     9ea2fec7b41a829638c8917bc3fe7134a76469596c6a42561b26182e6f484973
```

"result"."execution_results"[0]."transfers[0]": `transfer-e7ecec5d29c5045399bb93fa1a5868cd7fdacdb05f889a20a54f10d2eddba56f`

"result"."execution_results"[0]."block_hash": `9c32ac5bca7034192f6de6f10085de9be58edda9cb0dccf8b50a758c921e3f62`

Get state root hash:
```
casper-client get-block \
      --id 3 \
      --node-address http://157.230.49.238:7777 \
      --block-identifier 9c32ac5bca7034192f6de6f10085de9be58edda9cb0dccf8b50a758c921e3f62
```

"result"."block"."header"."state_root_hash": `a61579b22164a919212a65d5866110e741db9a7c13aa22339e6eeccc06b14ef6`

Query the Source Account:
```
casper-client query-state \
  --id 4 \
  --node-address http://157.230.49.238:7777 \
  --state-root-hash a61579b22164a919212a65d5866110e741db9a7c13aa22339e6eeccc06b14ef6 \
  --key 01e456c3779510fd14e83fa3be84ff4b2a22de76ef6be677ed7936f37f7712a0a4
```

"result"."stored_value"."Account"."main_purse": `uref-904bf9e4268486082d950c06ffcb5ff684b3d4547d8199a819248d1ab3f18c86-007`

Query the Target Account:
```
casper-client query-state \
      --id 5 \
      --node-address http://157.230.49.238:7777 \
      --state-root-hash a61579b22164a919212a65d5866110e741db9a7c13aa22339e6eeccc06b14ef6 \
      --key 0172a54c123b336fb1d386bbdff450623d1b5da904f5e2523b3e347b6d7573ae80
```

"result"."stored_value"."Account"."main_purse": `uref-e7ecec5d29c5045399bb93fa1a5868cd7fdacdb05f889a20a54f10d2eddba56f-007`

Get Source Account Balance:
```
casper-client get-balance \
      --id 6 \
      --node-address http://157.230.49.238:7777 \
      --state-root-hash a61579b22164a919212a65d5866110e741db9a7c13aa22339e6eeccc06b14ef6 \
      --purse-uref uref-904bf9e4268486082d950c06ffcb5ff684b3d4547d8199a819248d1ab3f18c86-007
```
```
{
  "id": 6,
  "jsonrpc": "2.0",
  "result": {
    "api_version": "1.3.2",
    "balance_value": "997489990000",
    "merkle_proof": "[19786 hex chars]"
  }
}
```

Get Target Account Balance:
```
casper-client get-balance \
      --id 7 \
      --node-address http://157.230.49.238:7777 \
      --state-root-hash a61579b22164a919212a65d5866110e741db9a7c13aa22339e6eeccc06b14ef6 \
      --purse-uref uref-e7ecec5d29c5045399bb93fa1a5868cd7fdacdb05f889a20a54f10d2eddba56f-007
```
```
{
  "id": 7,
  "jsonrpc": "2.0",
  "result": {
    "api_version": "1.3.2",
    "balance_value": "2500000000",
    "merkle_proof": "[20124 hex chars]"
  }
}
```

# 5. Delegate and Undelegate on the Casper Testnet

Account public hex-encoded key: `01e456c3779510fd14e83fa3be84ff4b2a22de76ef6be677ed7936f37f7712a0a4`

Validator: `017d96b9a63abcb61c870a4f55187a0a7ac24096bdb5fc585c12a686a4d892009e`

1. Delegate

![18](https://user-images.githubusercontent.com/21956707/134193078-0f4862d7-1148-4497-9172-04f3e9ccfd66.png)
![19](https://user-images.githubusercontent.com/21956707/134193096-d1fb69ba-61fc-406a-ab73-0df5ed4d3156.png)
![20](https://user-images.githubusercontent.com/21956707/134193105-04b8eba5-601b-41d4-85d1-bf66384dd947.png)

2. Undelegate

![21](https://user-images.githubusercontent.com/21956707/134193182-a05975dc-f798-4df3-8470-e14eee90a0ba.png)
![22](https://user-images.githubusercontent.com/21956707/134193189-2279402f-ec94-4a01-b075-1f22027f0d74.png)
![23](https://user-images.githubusercontent.com/21956707/134193208-a87e921a-7698-4136-a7c7-e8dbdd538d8b.png)

