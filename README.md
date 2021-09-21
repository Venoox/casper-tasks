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
