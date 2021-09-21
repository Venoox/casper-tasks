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
