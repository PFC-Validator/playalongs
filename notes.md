# development keys
do not use any of these phrases in the production environment

## admin

* address secret1yl2wklurhfxe5036y8ukszvl0a2adru0lend2v
* pubkey {"@type":"/cosmos.crypto.secp256k1.PubKey","key":"Alh6b8l4QSHSWuG1tt9dprlopTQemexc9b1x50UTyMuw"}
* mnemonic  ```myself odor trend soup rule mixture diagram move grape fiber novel shift chest buffalo fox can retire valley machine company suggest save runway dad```

## DM

* address secret1tcn06pv92u2qur42zp2pe5qq4d2p0ycgz748yf
* pubkey {"@type":"/cosmos.crypto.secp256k1.PubKey","key":"Ao0LlrdABuU8VTwc1ECQtZTJ6RPqb6gYqN7W/rxQsXNi\"}
* mnemonic: ```puppy tray poverty guide doll pond cereal street whip unique convince two attitude robust evoke harvest useful upset repeat camera outdoor vessel cash rare```

## player 1
* address secret10t98nr4s7cj53vtzpfrfzh062nfqex35fkfh90
* pubkey {"@type":"/cosmos.crypto.secp256k1.PubKey","key":"Aop2xeOdGYR2AA+7MxgSamul2Hu5Wsrgll0ILrmDZ2sW\"}
* mnemonic: ```dove raise fog garlic require insect beyond size tilt frame gold under square amount define mosquito color giant secret flat enter luxury vehicle put```

## player 2
* address secret167plw2qtexa9zchkxcdjccftkcgdwpplnx3jq2
* pubkey {"@type":"/cosmos.crypto.secp256k1.PubKey","key":"A17EzyEI7yppctzWk1BnJCR8pm243HAwmT4CsDHVc3rZ\"}
* mnemonic: ```term weird quarter wrong feed feature diesel boring picture hidden venture very stamp learn mother energy fat nominee creek oyster empty proof chapter call```

## NPC 1
* address secret12w4nzpmz6nrqwg5tp4h5wasc9xm4ralahmhcxr
* pubkey {"@type":"/cosmos.crypto.secp256k1.PubKey","key":"AkeDZVoBPhV+TWZ07gjwDiDS7ra2K1x6UGMKXR9tYKL0\"}
* mnemonic: ```symbol include pumpkin credit wealth sign swift clarify orient derive ketchup test wage modify industry elder twist music hill intact brief aunt work limit```

## General public 
* address secret1xxtcxk5jnt90n4hex2tkzmjmyep86ys7z6c4vs
* pubkey {"@type":"/cosmos.crypto.secp256k1.PubKey","key":"AjGx4JvV1xTmPwBe5bEq4v/IqK0miwoOlHidskM6bbC0\"}
* mnemonic: ```print army screen galaxy below uniform harsh mesh double mushroom dawn praise fortune ready layer lyrics island surprise spend vapor solve link have error```



# Deployment.
```shell
secretcli tx compute store contract.wasm.gz --from admin --gas 3000000 -y
tx= .. hash ..
secretcli query tx $tx|jq -e .raw_log
code_id= ..code_id value..
INIT='...'

secretcli tx compute instantiate $code_id "$INIT" --from admin --label playalong_admin --gas 1000000 -y 
secretcli query tx $tx|jq -e .raw_log
... (look at wasm/value for smart contract address)
DM_ADDRESS="secret196heewq9jky8rh976g5untqn3sfgzvzccz032p"
#player_2
PLAYER_ADDRESS="secret17mddraj4dreav6ek4tlrmcddhzvzjwtqfujwgp"
# NPC_2
NPC_ADDRESS="secret1hrwsef8d9mzjscmfqfqlqusxx7z5a0p6rszkds"

secretcli tx compute execute --label playalong_admin $MINT_DM --from admin -y 
```

## DM NFT
admin is owner of the DMs
```shell
INIT='
{
        "name": "playalong_admin",
        "symbol": "PLAY_ADMIN",
        "admin": "secret1yl2wklurhfxe5036y8ukszvl0a2adru0lend2v",
        "entropy": "string_used_as_entropy_when_generating_random_viewing_keys",
        "royalty_info": {
                "decimal_places_in_rates": 4,
                "royalties": [
                        {
                                "recipient": "secret1yl2wklurhfxe5036y8ukszvl0a2adru0lend2v",
                                "rate": 100
                        }
                ]
        },
        "config": {
                "public_token_supply": true ,
                "public_owner": true ,
                "enable_sealed_metadata":  false,
                "unwrapped_metadata_is_private":  false,
                "minter_may_update_metadata": true,
                "owner_may_update_metadata": true ,
                "enable_burn": true
        },
        "post_init_callback": null
}
```
```shell

MINT_DM=`
{
    "mint_nft": {
            "token_id": "69420",
            "owner": "secret1tcn06pv92u2qur42zp2pe5qq4d2p0ycgz748yf",
            "public_metadata": {
                    "token_uri": null,
                    "extension": {
                            "name": "GJ",
                            "game":"O Conners"
                    }
            },
            "private_metadata": {
                    "token_uri": null,
                    "extension": {
                            "field1": "value1"
                    }
            },
            "serial_number": {
                    "mint_run": 1,
                    "serial_number": 67,
                    "quantity_minted_this_run": 1
            },
            "royalty_info": {
                    "decimal_places_in_rates": 4,
                    "royalties": [
                            {
                                    "recipient": "secret1yl2wklurhfxe5036y8ukszvl0a2adru0lend2v",
                                    "rate": 100
                            }
                    ]
            },
            "memo": "PLAYALONGS",
            "padding": "optional_ignored_string_that_can_be_used_to_maintain_constant_message_length"
    }
}
`
```
## Player NFT
DM is owner of the players
```shell
INIT='
{
        "name": "playalong_player",
        "symbol": "PLAY_PLAYER",
        "admin": "secret1tcn06pv92u2qur42zp2pe5qq4d2p0ycgz748yf",
        "entropy": "entropy_player",
        "royalty_info": {
                "decimal_places_in_rates": 4,
                "royalties": [
                        {
                                "recipient": "secret1yl2wklurhfxe5036y8ukszvl0a2adru0lend2v",
                                "rate": 100
                        }
                ]
        },
        "config": {
                "public_token_supply": false ,
                "public_owner": false ,
                "enable_sealed_metadata":  true,
                "unwrapped_metadata_is_private":  true,
                "minter_may_update_metadata": true,
                "owner_may_update_metadata": true ,
                "enable_burn": true
        },
        "post_init_callback": null
}
'
```
## NPC NFT
DM is owner of the NPCs
```shell
INIT='
{
        "name": "playalong_player",
        "symbol": "PLAY_PLAYER",
        "admin": "secret1tcn06pv92u2qur42zp2pe5qq4d2p0ycgz748yf",
        "entropy": "entropy_NPC",
        "royalty_info": {
                "decimal_places_in_rates": 4,
                "royalties": [
                        {
                                "recipient": "secret1yl2wklurhfxe5036y8ukszvl0a2adru0lend2v",
                                "rate": 100
                        }
                ]
        },
        "config": {
                "public_token_supply": false ,
                "public_owner": true ,
                "enable_sealed_metadata":  true,
                "unwrapped_metadata_is_private":  true,
                "minter_may_update_metadata": true,
                "owner_may_update_metadata": true ,
                "enable_burn": true
        },
        "post_init_callback": null
}
'
```
